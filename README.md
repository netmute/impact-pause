# Pause Plugin for ImpactJS

This was originally written by Jesse Freeman. I just cleaned it up a bit.

## Usage

    ig.module(
      'game.main'
    )
    .requires(
      'impact.game',
      'plugins.pause'
    )
    .defines(function(){
      MyGame = ig.Game.extend({

        init: function() {
          ig.input.bind( ig.KEY.P, 'pause' );
        },

        update: function() {
          if ( ig.input.state('pause') ) {
            this.togglePause();
          }
        }
      })
    })

## How does it work?

We're giving the impression of stopping the game by not calling `update()` on the entities currently present in the game.

You can override this behavior for individual entities by adding the `ignorePause: true` attribute to the entity.

    SomeEntity = ig.Entity.extend({
        ignorePause: true
    })


## A note on timers

Since we're just skipping `Entity.update()`, all timers keep running while the game is paused. But sometimes you want certain timers to be stopped as well.

The plugin injects two hooks into `ig.Game` which you can use to fix this.

    onPause: function() {
      yourTimer.pause();
    },

    onResume: function() {
      yourTimer.unpause();
    }
