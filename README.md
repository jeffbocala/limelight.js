# Limelight

A tiny (and fast) JavaScript plugin that creates a spotlight around any element on the page.

## About

Most tooltip-tours include a blackout-style overlay that 'shines' a light on the target element. The problem is that it can be extremely hard to do that well. Traditionally you have two options:

- Add an overlay and promote the target's z-index. This is pretty good, but it relies on the target having a background, or it'll just be placed over the overlay. The element also has to have positioning for the z-index to even work (even if it's just relative, this can mess with stuff).
- Use an SVG mask. This works great, and was actually my first pass while creating this lib. However performance while animating (or even just repositioning) the mask was terrible in everything other than Blink, and Quantum doesn't event support transitions on mask transformations at all. Probably the best idea but not a great experience.
- Create 4 boxes that surround the target and transform them appropriately. A solid approach, but can get super complex if there are multiple tarets to 'cut out' of the backdrop. Also you're positioning 4 boxes simultaneously.

However I saw a really neat implementation in [react-joyride](https://github.com/gilbarbara/react-joyride) that uses `mix-blend-mode`. Nothing is 'cut out' of the overlay, and the element still stils beneath it. However we can position a div on top of the area we want to make transparent, and use `hard-light` to give the appearance it's showing through.

### Caveats

- Absolutely positioned elements within the target (e.g. a tooltip) will not be visible, since we can't include them in the target calculations. This is where the z-index approach has us beat.
- There needs to be a site wrapper (just a div after `body` is fine) that has a background-color, even if it's just white. Otherwise there's nothing to blend with. Weirdly it doesn't always work just putting it on `body` directly.

### Cool stuff

- Auto-adjusts if the height of the target changes. Since the RAF loop is run continuously while the overlay is open, any changes to the target height will be recalculated. This seems a bit scary from a perf perspective, but in reality is pretty quick.
