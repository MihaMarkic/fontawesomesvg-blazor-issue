# FontAwesome SVG & Blazor slow rendering

Using Blazor and FontAwesome SVG technology.

When there is a bigger document change, FontAwesome would trigger many times search through the DOM object and this heavily impacts rendering performance, like 50x times slower. It works fine when using FontAwesome with Web Fonts technology.

## Repro

TestGrid.razor creates bigger DOM content
```razor
@for (int i = 0; i < 5000; i++)
{
    <div>123</div>
}
```

When FontAwesome SVG script is present, the rendering is slow.
Run the app, click twice on Toggle grid button, rendering time will be written below button.

Then comment out FontAwesome script inclusion in _Layout.html file, and retry. Rendering will be fast.

On my machine the times are like 4,000ms vs 80ms

## Update with fix
Fixed by disabling FontAwesome mutations observation by placing script

```javascript
FontAwesomeConfig = {
    observeMutations: false
}
```

into head node of _Layout.cshtml (or just uncomment it).

Leaving project archived as I hope it helps to somebody else. Also note that you'd need to use your own FontAwesome script (replace FACODE with yours).