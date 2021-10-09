## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/Andrea-MariaDB-2/artsy.github.io/edit/pepopowitz/front-end-tour/docs/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown


# https://www.youtube.com/channel/UCzTBP3CKnn_Pfn9x1ESaOZA
## const canvas = document.getElementById("c");

const width = Math.min(window.innerWidth, 800);
const height = Math.min(window.innerHeight, 800);

const gpu = new GPU({
  canvas: canvas,
  mode: 'gpu'
});

function popcorn(x, y, h, t0, t1, t2, t3) {
  
  const N = 12;
  for (let i = 0; i < N; i++) {
    x = x - h * Math.sin(t2 + x + Math.atan((t1 + t3 / y / Math.PI)) / t0);
    y = y - h * Math.sin(t0 + y + Math.atan((t3 + t1 / x / Math.PI)) / t2);
    
    x = x - h * Math.cos(t0 + x -  Math.cos(t1 + y * Math.PI) + Math.sin(t2 + y * Math.PI * 2) * y * 0.5);
    y = y - h * Math.cos(t2 + y - Math.cos(t3 + x * Math.PI) + Math.sin(t0 + x * Math.PI * 2) * x * 0.5);
  }
  return [x, y]
}
gpu.addFunction(popcorn);

const calculate = gpu.createKernel(function(width, height, scale, param) {
  let sum = 0;
  let x = Math.floor(this.thread.x / scale) * scale;
  let y = Math.floor(this.thread.y / scale) * scale;
  
  // normalize (0..1)
  let xn = x / width;
  let yn = y / height;
  
  // mirror
  if (xn > 0.5) xn = 1 - xn
  
  // zoom
  let zoom = 8.1;
  let xz = xn * zoom
  let yz = yn * zoom
  let r = [xz, yz];
  
  // changes the contrast, not good to changee while running
  let h = 0.04;
  let t0 = 13 + Math.tan(param + 5)*3;
  let t1 = 32.1 + Math.atan(param/12)*3;
  let t2 = 7.1 + Math.cos(param) * 30;
  let t3 = 3.6;
  
  r = popcorn(r[0], r[1], h, t0, t1, t2, t3);
  let r0 = [r[0], r[1]]
  r = popcorn(r[0], r[1], h, t0, t1, t2, t3);
  r = popcorn(r[0], r[1], h, t0, t1, t2, t3);
  
  let rx = r[0]
  let ry = r[1]
  
  let v0 = Math.cos(r0[0]);
  let v1 = Math.cos(r0[1]);
  let v2 = Math.cos(r[0]) * param + Math.cos(r[1]) * (1- param);
  let v3 = 1 - Math.cos(r0[0]) + Math.cos(r0[1]);
  this.color(
    v3 * 0.69 + v2 * 0.81 - v0 * 0.3,
    v3 * 0.61 + v2 * 0.86 - v0 * 0.35,
    v3 * 0.64 + v2 * 0.83 - v0 * 0.22,
    1
  );
}).setOutput([width, height]).setGraphical(true);


let param = 1;

const draw = () => {
  let exp = [];
	let paramNorm;
  function update() {
    param = (param + 0.007) % (Math.PI * 2)
    paramNorm = Math.cos(param) /2 + 0.5
    calculate(width, height, 1, paramNorm);
    setTimeout(update, 50)
  }
  window.requestAnimationFrame(update);
};

draw()

### '<script src="https://unpkg.com/gpu.js@2.1.0/dist/gpu-browser.min.js"></script>'


- body {
  padding: 0;
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

canvas{
  border: 1px solid #666;
  box-shadow:
    7px 3px 22px 0px rgba(0,0,0,0.4),
    2px 2px 4px 0px rgba(0,0,0,0.3);
}

- '<script src="unpkg.com/gpu.js@2.1.0/dist/gpu-browser.min.js">'

- '<canvas id="c" width="800" height="800"></canvas>'


**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/Andrea-MariaDB-2/artsy.github.io/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
