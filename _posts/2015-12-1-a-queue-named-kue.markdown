---
layout: post
title:  "A Queue called Kue"
date:   2015-12-1 18:30:00
categories: javascript kue queue
disqus: true
---

Let's say you want to perform some job at a time in the future. This could be something as simple as sending an email, or something more complicated like manipulation of video files. Normally, CRON jobs would do the trick. However, on distributed systems like Heroku, they can be a major [pain in the butt](https://devcenter.heroku.com/articles/scheduled-jobs-custom-clock-processes) due to process restarts and a [temporary filesystem](https://devcenter.heroku.com/articles/dynos#ephemeral-filesystem). [Kue.js](https://github.com/Automattic/kue) is a priority job queue that is backed by a Redis instance, and thus solves at least one problem of persistence for us.

This is how you create a connection to the Singleton Queue instance on redis:

    var queue = kue.createQueue(kueOptions);

All processes that utilize the job queue will need to perform this step. Depending on the kueOptions object passed in, we will connect to a redis instance (possibly a redis addon if we are on Heroku). If no options are passed in, kue uses the default local redis configurations.


#### Creating jobs
The first thing you'll probably want to do is create a job to be processed at a later date. The following is an example of creating a job with type 'email':

    // Create an email job for later
    var job = queue.create('email', {
      to: email,
      from: 'rmindrdev@gmail.com',
      message: message,
      datetime: datetimeUTC,
    })
    // jobDelay calculated elsewhere, number of ms or Date obj representing when the job should be processed
    .delay(jobDelay)
    .save(function(err) {
      if (err) console.log(err);
    });

This job object is stored in the Redis instance with some additional fields that kue uses to determine it's status and what to do with it upon retrieval.

#### Events
Kue makes it possible to add events at the job-level or queue level. Setting these events is syntactically similar to jQuery, Backbone, and many other libararies. This is how you would set a completion task on the job we just created above:

    job.on('complete', function(result){
      console.log('Job completed with data ', result);
    });

It's important to note that if your project is deployed on distributed systems like Heroku (especially if you're not paying enough for guaranteed uptime), job events should not be considered reliable. When a process restarts, we lose reference to the specific Job object that we are dealing with since it is stored in memory, however, queue level events are stored on the Singleton Queue which offers slightly less flexibility but significantly more reliability. The following queue-level event roughly matches the job-level event shown previously:

    .on('job complete', function(id, result) {
      console.log('Job', id, 'completed with result', result);
    });


#### Processing
When a job is ready to be performed (immediately if you do not provide a job delay upon creating it), kue will trigger an event it will be promoted from `delayed` to `queued`, at which point a running process with the following code, will process it:

    queue.process('email', function(job, done) {
      console.log('Processing job #', job.id, ':\n', job.data);
      sendEmail(job.data.to, job.data.message, done);
    });

Boom. Job complete.

#### Conclusion
  So far, my experiences with Kue have been very positive, although it can be tricky to get it all working correctly in a deployed, distributed environment. It is definitely a good object when CRON jobs alone will not suffice. I am currently using Kue.js in [rmindr](https://github.com/dougshamoo/rmindr), which is obviously the coolest thing since sliced bread or rollerblades.
