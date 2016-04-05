# Building an Infinite Scroll Ember CLI Addon


Back in June, Ember CLI introduced the concept of addons. Since then, I have gotten in the habit of extracting common functionality from my Ember apps and moving it into these addons. This has proven to be incredibly powerful. Now, when starting up a new Ember project, I dont need to deal with copy/pasting or any other less-than-optimal way of sharing code between applications. With addons, I just `npm install` and I am on my way.

One piece of functionality that I recently extracted was an infinite scroll component. I thought that the process of doing this could be instructive for others just starting out with Ember CLI addons.

For the purposes of this post, I am going to assume that you are familiar with the basics of addons and what they allow you to do. If this is not the case, I suggest watching [Richard Livsey’s](https://twitter.com/rlivsey) fantastic [introductory video](http://vimeo.com/109672719) before you continue here.

With that out of the way, let’s get started by generating the addon skeleton. With Ember CLI installed:



    ember addon ember-infinite-scroll



The lone export of this addon will be an `infinite-scroll` component:



    1 // addon/components/infinite-scroll.js
    2 
    3 import Em from 'ember';
    4 
    5 export default Em.Component.extend({
    6   // TODO
    7 });



We will flesh this out in a moment, but first, a quick aside about how ember applications interact with addons. The way our addon is currently constructed, we would need to manually import the component in order to use it. We can make the component automatically available by simply putting it in the app directory rather than the addon directory. However, putting all of the component code in the app directory makes it impossible for users to modify the component. For this reason, it is best to put the actual code in the addon directory and then simply import and export that code within the app directory:



    1 // app/components/infinite-scroll.js
    2 
    3 import InfiniteScroll from 'ember-infinite-scroll/components/infinite-scroll';
    4 
    5 export default InfiniteScroll;



In order to better understand the code, it helps to see how it will be used from within the consuming application. In a template, at the end of the list of items you want to make infinitely scrollable, you add:



    1 {{#infinite-scroll content=model hasMore=hasMore}}
    2   <span>Loading...</span>
    3 {{/infinite-scroll}}



Then, within your route/controller/component, you add the following action:



    1 fetchMore: function(callback) {
    2   var promise = this.fetchMoreItems();
    3   callback(promise);
    4 }



At a high level, the component works like this… It listens for scroll events and if it determines that the user has scrolled to the bottom of the page, it will invoke the `fetchMore` action above. The action is responsible for forming the correct request to fetch more items. This request must be in the form of a promise and this promise must then be passed back into the component by invoking the provided callback. The component will then take the promise and append the resolved items onto the existing content.

Let’s first just setup the scroll listener:



     1 // addon/components/infinite-scroll.js
     2 
     3 import Em from 'ember';
     4 
     5 var $window = Em.$(window),
     6     bind = Em.run.bind;
     7 
     8 export default Em.Component.extend({
     9   hasMore: null,
    10   content: null,
    11 
    12   setup: function() {
    13     $window.on('scroll', bind(this, this.didScroll));
    14   }.on('didInsertElement'),
    15 
    16   teardown: function() {
    17     $window.off('scroll', bind(this, this.didScroll));
    18   }.on('willDestroyElement'),
    19 
    20   didScroll: function() {
    21   	// TODO
    22   }
    23 });



Note that we are using the convenient [Ember.run.bind](http://emberjs.com/api/classes/Ember.run.html#method_bind) utility in order to wrap the event handler in a run-loop.

Ok great, our component is now setup to listen for scroll events on the window and to delegate the handling of those events to a yet-to-be-implemented `didScroll` method. Within this `didScroll` method, we ultimately want to fetch more items, but only if the user is at the bottom of the page AND there are actually more items AND we are not already fetching them:



     1 // addon/components/infinite-scroll.js
     2 
     3 import Em from 'ember';
     4 
     5 var $window = Em.$(window),
     6     bind = Em.run.bind;
     7 
     8 export default Em.Component.extend({
     9   action: 'fetchMore',
    10   isFetching: false,
    11   hasMore: null,
    12   content: null,
    13 
    14   setup: function() {
    15     $window.on('scroll', bind(this, this.didScroll));
    16   }.on('didInsertElement'),
    17 
    18   teardown: function() {
    19     $window.off('scroll', bind(this, this.didScroll));
    20   }.on('willDestroyElement'),
    21 
    22   didScroll: function() {
    23     if (this.isNearBottom() && this.get('hasMore') && !this.get('isFetching')) {
    24       this.set('isFetching', true);
    25       this.sendAction('action', bind(this, this.handleFetch));
    26     }
    27   },
    28 
    29   handleFetch: function(promise) {
    30     // TODO
    31   },
    32 
    33   isNearBottom: function() {
    34     // TODO
    35   }
    36 });



Note that we are setting an `isFetching` flag in order to prevent our component from attempting to fetch more items when a fetch is already in progress. This flag also allows us to display a loading indicator. In order to do this, we need to add the basic template:



    1 {{!-- app/templates/components/infinite-scroll.hbs --}}
    2 {{#if isFetching}}{{yield}}{{/if}}



While we are fetching more, the component yields, allowing the consuming application to display whatever type of loding indicator it wants. When it is not fetching, it shows nothing.

Alright. The basics are all there now, but we need to fill in the stubbed out `handleFetch` and `isNearBottom`:



     1 // addon/components/infinite-scroll.js
     2 
     3 import Em from 'ember';
     4 
     5 var $window = Em.$(window),
     6     $document = Em.$(document),
     7     bind = Em.run.bind;
     8 
     9 var EPSILON = 150;
    10 
    11 export default Em.Component.extend({
    12   action: 'fetchMore',
    13   epsilon: EPSILON,
    14   isFetching: false,
    15   hasMore: null,
    16   content: null,
    17 
    18   setup: function() {
    19     $window.on('scroll', bind(this, this.didScroll));
    20   }.on('didInsertElement'),
    21 
    22   teardown: function() {
    23     $window.off('scroll', bind(this, this.didScroll));
    24   }.on('willDestroyElement'),
    25 
    26   didScroll: function() {
    27     if (this.isNearBottom() && this.get('hasMore') && !this.get('isFetching')) {
    28       this.set('isFetching', true);
    29       this.sendAction('action', bind(this, this.handleFetch));
    30     }
    31   },
    32 
    33   handleFetch: function(promise) {
    34     var success = bind(this, this.fetchDidSucceed),
    35         failure = bind(this, this.fetchDidFail);
    36 
    37     promise.then(success, failure);
    38   },
    39 
    40   fetchDidSucceed: function(items) {
    41     this.set('isFetching', false);
    42     this.get('content').pushObjects(items);
    43   },
    44 
    45   fetchDidFail: function() {
    46     this.set('isFetching', false);
    47   },
    48 
    49   isNearBottom: function() {
    50     var viewPortTop = $document.scrollTop(),
    51         bottomTop = ($document.height() - $window.height());
    52 
    53     return viewPortTop && (bottomTop - viewPortTop) < this.get('epsilon');
    54   }
    55 });



`handleFetch` is pretty simple. Regardless of whether the promise resolves or rejects, we reset the `isFetching` flag. Then, in the case where it resolves successfully, we append the new items to the existing content.

`isNearBottom` requires a little more explanation. First, we grab the vertical position of the document’s scroll bar (`viewPortTop`). Then we grab the vertical position at which the page document will be scrolled all the way to the bottom (`bottomTop`). We return false if we are at the top of the page (`viewPortTop === 0`) or if `viewportTop` is not within a reasonable distance of `bottomTop`. Otherwise we return true.

That’s really all there is to it. One more change can be made in order to make this component more user-friendly in the common scenario where the application uses Ember Data. In this case, the promise that gets passed back into `handleFetch` will be a `DS.RecordArray`. We can’t just append this to the existing content. Instead, we need to append this `RecordArray`’s content onto the existing content. This just requires a small change in `fetchDidSucceed`. The final component looks like this:



     1 // addon/components/infinite-scroll.js
     2 
     3 import Em from 'ember';
     4 
     5 var $window = Em.$(window),
     6     $document = Em.$(document),
     7     bind = Em.run.bind;
     8 
     9 var EPSILON = 150;
    10 
    11 export default Em.Component.extend({
    12   action: 'fetchMore',
    13   epsilon: EPSILON,
    14   isFetching: false,
    15   hasMore: null,
    16   content: null,
    17 
    18   setup: function() {
    19     $window.on('scroll', bind(this, this.didScroll));
    20   }.on('didInsertElement'),
    21 
    22   teardown: function() {
    23     $window.off('scroll', bind(this, this.didScroll));
    24   }.on('willDestroyElement'),
    25 
    26   didScroll: function() {
    27     if (this.isNearBottom() && this.get('hasMore') && !this.get('isFetching')) {
    28       this.set('isFetching', true);
    29       this.sendAction('action', bind(this, this.handleFetch));
    30     }
    31   },
    32 
    33   handleFetch: function(promise) {
    34     var success = bind(this, this.fetchDidSucceed),
    35         failure = bind(this, this.fetchDidFail);
    36 
    37     promise.then(success, failure);
    38   },
    39 
    40   fetchDidSucceed: function(response) {
    41     var content = Em.get(response, 'content') || response;
    42     this.set('isFetching', false);
    43     this.get('content').pushObjects(content);
    44   },
    45 
    46   fetchDidFail: function() {
    47     this.set('isFetching', false);
    48   },
    49 
    50   isNearBottom: function() {
    51     var viewPortTop = $document.scrollTop(),
    52         bottomTop = ($document.height() - $window.height());
    53 
    54     return viewPortTop && (bottomTop - viewPortTop) < this.get('epsilon');
    55   }
    56 });


That’s it! Hopefully this walkthrough helped illustrate just how powerful Ember CLI addons can be. If you want to take a further look, you can check this out on [github](https://github.com/jasonkriss/ember-infinite-scroll). Also, if you have any questions or comments at all, please leave them below or find me on twitter/email. Thanks for reading!

-------------------------------------------

原文：http://blog.jasonkriss.com/building-an-infinite-scroll-ember-cli-addon/
源码：https://github.com/jasonkriss/ember-infinite-scroll