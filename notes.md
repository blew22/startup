# CS 260 Notes

[My startup - Shared Budget](https://sharedbudget.click)

## Helpful links

- [Course instruction](https://github.com/webprogramming260)
- [MDN](https://developer.mozilla.org)

## AWS

My Elastic IP address is: 98.87.184.136
Web server is an EC2 instance using the class's AMI.
I've allocated an elastic IP to the instance so that the IP will stay the same even if I shutdown or restart the server.

Use the following to ssh into server: 
```
➜  ssh -i [key pair file location] ubuntu@[ip address]
```

Register a domain name through AWS Route 53. The .click TLD (top-level domain) costs $3/yr in Oct 2025. Go to `hosted zones > create records` to create records that tie your server's IP to the domain name.

## Caddy

### Learned to edit files from CL:

    vi filename
- **Entering Text:** Switch to insert mode by pressing `ins`, `i`, `a`, or `o`. Type your text.
- **Exiting Insert Mode:** Press the `Esc` key to return to command mode.
- **Saving and Exiting:** In command mode, type `:wq` and press Enter to save changes and quit.

### Edited Caddy config file

## HTML

Command to deploy files to server:
```bash
./deployFiles.sh -k [keyFileLocation] -h sharedbudget.click -s simon
```

I still don't quite get how form subissions and adding button functions works, but I guess we'll get there.

## CSS

All IDs in an html doc should be unique. So, making a CSS rule by ID should affect a specific element.

| Combinator       | Meaning                    | Example        | Description                                |
| ---------------- | -------------------------- | -------------- | ------------------------------------------ |
| Descendant       | A list of descendants      | `body section` | Any section that is a descendant of a body |
| Child            | A list of direct children  | `section > p`  | Any p that is a direct child of a section  |
| General sibling  | A list of siblings         | `div ~ p`      | Any p that has a div sibling               |
| Adjacent sibling | A list of adjacent sibling | `div + p`      | Any p that has an adjacent div sibling     |

### Animations
Define `animation-name` and `animation-duration` properties for the element that you want to animate. Then, use `@keyframe` to give start and end keyframes as well as optional keyframes in the middle.
```
@keyframes [animation-name] {
  from {
    [starting style/position]
  }
  50% {
    [intermediate style/position]
  }
  to {
    [ending style/position]
  }
}
```


### Responsive design
Mobile browsers scale websites automatically to work on a small screen. You need to override this if you want control of your website's responsive design.
```
<meta name="viewport" content="width=device-width,initial-scale=1" />
```

change or remove elements when the screen height is greater than width
```@media (orientation: portrait) {
  aside {
    display: none;
  }
}
```

CSS `display` property value options: `none`, `block` (default of p and div), `inline` (default of b and span), `flex`, `grid`

#### grids
"We turn a series of elements into a responsive grid by including a CSS `display` property with the value of `grid` on the container element. This tells the browser that all of the children of this element are to be displayed in a grid flow. The `grid-template-columns` property specifies the layout of the grid columns. We set this to repeatedly define each column to auto-fill the parent element's width with children that are resized to a minimum of 300 pixels and a maximum of one equal fractional unit (`1fr`) of the parents total width. A fractional unit is dynamically computed by splitting up the parent element's width into equal parts. Next, we fix the height of the rows to be exactly 300 pixels by specifying the `grid-auto-rows` property. Finally, we finish off the grid configuration by setting the `grid-gap` property to have a gap of at least 1 em between each grid item."

```
.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  grid-auto-rows: 300px;
  grid-gap: 1em;
}
```


I did like the navbar it made it super easy to build a responsive header.

```html
      <nav class="navbar navbar-expand-lg bg-body-tertiary">
        <div class="container-fluid">
          <a class="navbar-brand">
            <img src="logo.svg" width="30" height="30" class="d-inline-block align-top" alt="" />
            Calmer
          </a>
          <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent">
            <span class="navbar-toggler-icon"></span>
          </button>
          <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <ul class="navbar-nav me-auto mb-2 mb-lg-0">
              <li class="nav-item">
                <a class="nav-link active" href="play.html">Play</a>
              </li>
              <li class="nav-item">
                <a class="nav-link" href="about.html">About</a>
              </li>
              <li class="nav-item">
                <a class="nav-link" href="index.html">Logout</a>
              </li>
            </ul>
          </div>
        </div>
      </nav>
    </header>
```

I also used SVG to make the icon and logo for the app. This turned out to be a piece of cake.

```html
<svg width="100" height="100" xmlns="http://www.w3.org/2000/svg">
  <rect width="100" height="100" fill="#0066aa" rx="10" ry="10" />
  <text x="50%" y="50%" dominant-baseline="central" text-anchor="middle" font-size="72" font-family="Arial" fill="white">C</text>
</svg>
```

## React Part 1: Routing

Setting up Vite and React was pretty simple. I had a bit of trouble because of conflicting CSS. This isn't as straight forward as you would find with Svelte or Vue, but I made it work in the end. If there was a ton of CSS it would be a real problem. It sure was nice to have the code structured in a more usable way.

## React Part 2: Reactivity

This was a lot of fun to see it all come together. I had to keep remembering to use React state instead of just manipulating the DOM directly.

Handling the toggling of the checkboxes was particularly interesting.

```jsx
<div className="input-group sound-button-container">
  {calmSoundTypes.map((sound, index) => (
    <div key={index} className="form-check form-switch">
      <input
        className="form-check-input"
        type="checkbox"
        value={sound}
        id={sound}
        onChange={() => togglePlay(sound)}
        checked={selectedSounds.includes(sound)}
      ></input>
      <label className="form-check-label" htmlFor={sound}>
        {sound}
      </label>
    </div>
  ))}
</div>
```
