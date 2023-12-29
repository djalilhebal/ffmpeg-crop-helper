# Ramblings

Ideas:
- ffmpeg crop visually
- ffmpeg-crop-semiautomatically


## TODO

- [x] Scaffold project (Vite, Vue, JavaScript)

- [x] Add Tailwind, following https://tailwindcss.com/docs/guides/vite#vue


## Impl notes

- About the [`load` event][html-load-event]:
    * "The load event fires for elements containing a resource when the resource has successfully loaded."
    * KAITO: It triggers whenever the `img.src` loads. It re-triggers if we assign `src` to the current value.

- Thought of using [CSS `resize`][css-resize], but I would still need to handle moving.

- We could make it moveable using arrow keys, but that doesn't sound easy enough.

- [HTML `draggable` global attribute][html-draggable] doesn't seem very helpful.


[html-load-event]: https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/load_event
[css-resize]: https://developer.mozilla.org/en-US/docs/Web/CSS/resize
[html-draggable]: https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/draggable

---

END.
