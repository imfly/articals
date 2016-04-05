# Binding to scroll events with Ember.js

You can learn a lot by reading the discourse source code. Today I found a nice mixin which makes it easy to get a notification when the browser window is scrolled. Here's what I learned...

## Binding to browser scroll/touch events is easy:

    App.Scrolling = Em.Mixin.create({

      bindScrolling: function(opts) {
        var onScroll, _this = this;

        onScroll = function(){ 
            return _this.scrolled(); 
        };

        $(document).bind('touchmove', onScroll);
        $(window).bind('scroll', onScroll);
      },

      unbindScrolling: function() {
        $(window).unbind('scroll');
        $(document).unbind('touchmove');
      }

    });

## Using the above mixin, you can bind/unbind to scroll events from any view:

    App.MyView = Ember.View.extend(App.Scrolling, {
        didInsertElement: function() {
            this.bindScrolling();
        },
        willRemoveElement: function() {
            this.unbindScrolling();
        },
        scrolled: function() {
          console.log('MyView was scrolled')
        }
    });

## Scroll events probably need to be debounced!

Scroll events are triggered very quickly, so the above view's scroll() method is going to get called like crazy. The only sane way to do this is by **debouncing**. Discourse does this by adding a `debounce()` function to the application namespace. Assuming we have our own `App.debounce` the scrollable mixin can use it to throttle scroll notifications:

    App.Scrolling = Em.Mixin.create({

      bindScrolling: function(opts) {
        var onScroll, _this = this;
        opts = opts || {debounce: 100};

        if (opts.debounce) {
          onScroll = App.debounce(function() { return _this.scrolled(); }, 100);
        } else {
          onScroll = function(){ 
              return _this.scrolled(); 
          };
        }
        $(document).bind('touchmove', onScroll);
        $(window).bind('scroll', onScroll);
      },

      unbindScrolling: function() {
        $(window).unbind('scroll');
        $(document).unbind('touchmove');
      }

    });

You can find the original version (in the discourse github rebo)[[https://github.com/discourse/discourse/blob/master/app/assets/javascripts/discourse/mixins/scrolling.js](https://github.com/discourse/discourse/blob/master/app/assets/javascripts/discourse/mixins/scrolling.js)]

------------------------------

原文：https://coderwall.com/p/tes2cw/binding-to-scroll-events-with-ember-js