# CSS-8 — Orbit Animation with CSS Keyframes

## About This Project

This is my Laundry Wallah landing page from the previous assignment, but now with a CSS keyframe animation added to the washing machine image. The goal for CSS-8 was to make the image move in a circular orbit path using `@keyframes` and chained `transform` functions.

The page still has the header, hero section with a call-to-action button, and footer from before. The new part is the washing machine floating in a continuous circular orbit.

---

## How to View It

1. Download or clone this repo.
2. Make sure these files are in the same folder:
   - `index.html`
   - `styles.css`
   - `washingmachine.png`
3. Open `index.html` in any browser (Chrome, Firefox, Edge).
4. Watch the washing machine image — it orbits in a smooth circle.
5. Hover over the image area to see it scale up while still orbiting.

No build tools or servers needed, just HTML and CSS.

---

## What I Learned About CSS Keyframe Animations

### The Problem I Had at First

My first attempt used `translate()` to move the image up, then diagonally, then back down. It looked like it was bobbing around randomly, not orbiting. The feedback I got was that this is NOT orbital motion — it's just shifting the image in cardinal directions.

### How Circular Orbit Actually Works

The trick is chaining three transforms together:

```css
transform: rotate(Adeg) translateY(-15px) rotate(-Adeg);
```

Here's what's happening step by step:

1. `rotate(Adeg)` — rotates the entire coordinate system by angle A. This decides *where* on the circle the image sits.
2. `translateY(-15px)` — pushes the image 15px along the rotated Y axis. Since the axis itself is rotated, the image ends up at a different point on a circle of radius 15px.
3. `rotate(-Adeg)` — cancels out the first rotation so the image itself stays upright and doesn't spin.

So in the keyframes, I go from `rotate(0deg)` to `rotate(360deg)`, and the counter-rotation goes from `rotate(0deg)` to `rotate(-360deg)`:

```css
@keyframes washOrbit {
    0% {
        transform: rotate(0deg) translateY(-15px) rotate(0deg);
    }
    100% {
        transform: rotate(360deg) translateY(-15px) rotate(-360deg);
    }
}
```

The browser interpolates smoothly between 0 and 360 degrees, which traces out a perfect circle. I set `animation: washOrbit 6s linear infinite` so it loops forever at a constant speed.

### Fixing the Hover Conflict

I ran into another problem — when I tried to add `transform: scale(1.15)` on hover directly to the `img`, it overrode the orbit animation because CSS only allows one `transform` property at a time.

My fix was to put the hover scale on the **parent** `.hero-image` div instead:

```css
.hero-image {
    transition: transform 0.5s ease;
}
.hero-image:hover {
    transform: scale(1.15);
}
```

Since the scale is on the parent and the orbit is on the child `img`, they don't conflict. The image keeps orbiting while the whole container zooms in smoothly on hover.

---

## File Structure

```
CSS-8/
├── index.html          — Page structure (HTML)
├── styles.css          — Styling and orbit animation (CSS)
├── washingmachine.png  — Hero section image
└── README.md           — This file
```

---

## Technologies Used

- HTML5
- CSS3 (Keyframe Animations, Transforms, Transitions, Flexbox, Grid)
- Google Fonts (Poppins)
