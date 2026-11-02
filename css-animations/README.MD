# CSS Animations

A small teaching project that explains how CSS animations work.

## Files

| File | Purpose |
|------|---------|
| `css-animations-lesson.html` | Interactive lesson: 8 live demos with code, explanations and Replay buttons |
| `css-animations-corrected.html` | Corrected version of the original single-box example (slide + colour change + rotation) |

Open either file directly in a browser. No build step or dependencies.

---

## How an animation works

Every CSS animation has two parts:

1. **`@keyframes`**: describes *what* changes (the steps of the animation).
2. **`animation-*` properties**: describe *how* it plays (duration, speed curve, repeats, etc.).

```css
@keyframes slide {
  from { transform: translateX(0); }
  to   { transform: translateX(220px); }
}

.box {
  animation-name: slide;
  animation-duration: 2s;
}
```

> `animation-duration` defaults to `0s`, so an animation without a duration will not be visible.

---

## Animation properties

### 1. `animation-name`
The name of the `@keyframes` rule to use.

### 2. `animation-duration`
How long one cycle takes (`2s`, `500ms`).

### 3. `animation-timing-function`
The speed curve of one cycle.

| Value | Behaviour |
|-------|-----------|
| `linear` | Constant speed |
| `ease` | Slow start, fast middle, slow end (default) |
| `ease-in` | Slow start |
| `ease-out` | Slow end |
| `ease-in-out` | Slow start and end |
| `steps(n)` | Jumps in `n` discrete steps |

### 4. `animation-delay`
Wait time before the animation starts (`1s`).

### 5. `animation-iteration-count`
How many times it plays: a number (`3`) or `infinite`.

### 6. `animation-direction`

| Value | Behaviour |
|-------|-----------|
| `normal` | Plays forwards each time (jumps back to the start on repeat) |
| `reverse` | Plays backwards |
| `alternate` | Forwards, then backwards, and so on |
| `alternate-reverse` | Backwards, then forwards, and so on |

### 7. `animation-fill-mode`
What the element looks like **before** the animation starts and **after** it ends.

| Value | Behaviour |
|-------|-----------|
| `none` | Default. Element snaps back to its normal style after the animation |
| `forwards` | Keeps the last keyframe's styles after the animation ends |
| `backwards` | Applies the first keyframe's styles during the delay |
| `both` | Both of the above |

### 8. `animation-play-state`
`running` (default) or `paused`. Useful for pausing and resuming with JavaScript or `:hover`.

---

## Shorthand

```css
animation: name duration timing-function delay iteration-count direction fill-mode;
```

Example:

```css
animation: slide 1s ease-in-out 1s 3 alternate forwards;
```

The first time value is always the **duration** and the second is the **delay**.

---

## Multiple animations

Separate animations with commas:

```css
.box {
  animation:
    slide 2s ease-in-out forwards,
    colorChange 2s linear forwards,
    spin 2s linear forwards;
}
```

**Important:** if two animations change the same property (for example both use `transform`), the later one in the list wins and the earlier one is lost. Avoid this by using the individual transform properties:

| Instead of | Use |
|-----------|-----|
| `transform: translateX(...)` | `translate: ...` |
| `transform: rotate(...)` | `rotate: ...` |
| `transform: scale(...)` | `scale: ...` |

---

## Percentage keyframes

Use `0%` to `100%` for more than two steps:

```css
@keyframes bounce {
  0%   { transform: translateY(0); }
  30%  { transform: translateY(-30px); }
  60%  { transform: translateY(0); }
  80%  { transform: translateY(-12px); }
  100% { transform: translateY(0); }
}
```

`from` is the same as `0%` and `to` is the same as `100%`.

---

## Bugs fixed in the original code

| Problem | Fix |
|---------|-----|
| `transform: deg(220deg)` is not a valid CSS function and is ignored | Use `rotate(220deg)` |
| Both animations used `transform`, so the second overrode the first | Use the separate `translate` and `rotate` properties |
| No `animation-fill-mode`, so the box snapped back after the animation | Add `forwards` |
| Box had no width or height, so it stretched across the container | Give it a size |
| `translateX(500px)` was wider than the `30vw` container | Use `calc(30vw - 60px)` |

---

## Tips

- **Replaying an animation with JavaScript:** remove the class, force a reflow, then add it back.

  ```js
  el.classList.remove('run');
  void el.offsetWidth; // forces the browser to recalculate styles
  el.classList.add('run');
  ```
- **Performance:** animating `transform`, `translate`, `rotate`, `scale` and `opacity` is smooth because the browser can hand them to the GPU. Animating `width`, `height`, `top` or `left` is slower.
- **Accessibility:** respect users who prefer less motion.

  ```css
  @media (prefers-reduced-motion: reduce) {
    .box { animation-duration: 0.01s; }
  }
  ```

## Exercises for students

1. Change `ease-in-out` to `linear` and describe the difference.
2. Make the box slide back and forth forever.
3. Remove `forwards`. What happens at the end, and why?
4. Make the slide and the rotation play at the same time.
5. Create a pulse effect with `scale` and percentage keyframes.