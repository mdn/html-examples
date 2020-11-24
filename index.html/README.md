<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Custom &lt;select&gt; example</title>
    <style>
    body {
      font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif
    }

    .select:focus {
      border-color: blue;
    }

    html body form fieldset#custom div.select[data-multiple] div.header {
      display: none;
    }

    html body form fieldset#custom div.select div.header {
      content: '↓';
      display: -webkit-inline-box;
      display: -ms-inline-flexbox;
      display: inline-flex;
      -webkit-box-align: center;
      -ms-flex-align: center;
      align-items: center;
      padding: 0;
      position: relative;
    }

    html body form fieldset#custom div.select div.header::after {
      content: '↓';
      align-self: stretch;
      display: flex;
      align-content: center;
      justify-content: center;
      justify-items: center;
      align-items: center;
      padding: .5em;
    }

    html body form fieldset#custom div.select div.header:hover:after {
      background-color: blue;
    }

    .select .header select {
      -webkit-appearance: none;
      -moz-appearance: none;
      appearance: none;
      font-family: inherit;
      font-size: inherit;
      padding: 0;
      border-width: 0;
      width: 100%;
      flex: 1;
      display: none;
    }

    .select .header select optgroup {
      display: none;
    }

    .select select div.option {
      display: none;
    }

    html body form fieldset#custom div.select {
      user-select: none;
      box-sizing: border-box;
      position: relative;
      border-radius: 4px;
      border-style: solid;
      border-width: 0;
      border-color: gray;
      width: auto;
      display: inline-block;
    }

    html body form fieldset#custom div.select:focus {
      border-color: blue;
    }

    html body form fieldset#custom div.select:hover {
      border-color: blue;
    }

    html body form fieldset#custom div.select[data-open] {
      border-bottom-left-radius: 0;
      border-bottom-right-radius: 0;
    }

    html body form fieldset#custom div.select[data-open] datalist {
      display: initial;
    }

    html body form fieldset#custom div.select datalist {
      -webkit-appearance: none;
      -moz-appearance: none;
      appearance: none;
      position: absolute;
      border-style: solid;
      border-width: 1px;
      border-color: gray;
      left: 0;
      display: none;
      width: 100%;
      box-sizing: border-box;
      z-index: 2;
      border-bottom-left-radius: 4px;
      border-bottom-right-radius: 4px;
    }

    html body form fieldset#custom div.select datalist div.option {
      background-color: white;
      margin-bottom: 1px;
      cursor: pointer;
      padding: 0.5em;
      border-width: 0;
    }

    html body form fieldset#custom div.select datalist div.option:hover {
      background-color: blue;
      color: white;
    }

    html body form fieldset#custom div.select datalist div.option:focus {
      background-color: blue;
      color: white;
    }

    html body form fieldset#custom div.select datalist div.option:checked {
      background-color: blue;
      color: white;
    }

    html body form fieldset#custom div.select div.optgroup div.option[data-disabled] {
      color: gray;
    }

    html body form fieldset#custom div.select div.optgroup div.option[data-checked] {
      background-color: blue;
      color: white;
    }

    html body form fieldset#custom div.select div.optgroup div.label {
      font-weight: bold;
    }

    html body form fieldset#custom div.select div.optgroup div.option div.label {
      font-weight: normal;
      padding: .25em;
    }

    html body form fieldset#custom div.select div.header {
      flex: 1;
      display: flex;
      width: auto;
      box-sizing: border-box;
      border-width: 1px;
      border-style: inherit;
      border-color: inherit;
      border-radius: inherit;
    }

    html body form fieldset#custom div.select div.header span {
      flex: 1;
      padding: .5em;
    }
    </style>
  </head>
  <body>
    <form>
       <fieldset>
          <legend>Standard controls</legend>
          <select
             name=1A
             id=select
             autocomplete=off
             required
             >
             <option>Carrots</option>
             <option>Peas</option>
             <option>Beans</option>
             <option>Pneumonoultramicroscopicsilicovolcanoconiosis</option>
          </select>
       </fieldset>
       <fieldset id=custom>
          <legend>Custom controls</legend>
          <select
             name="2A"
             id="select"
             autocomplete="off"
             required
             >
             <option>Carrots</option>
             <option>Peas</option>
             <option>Beans</option>
             <option>Pneumonoultramicroscopicsilicovolcanoconiosis</option>
          </select>
        </fieldset>
      </form>
      <script>
      const selects = custom.querySelectorAll('select');
      for (const select of selects) {
          const div = document.createElement('div');
          const header = document.createElement('div');
          const datalist = document.createElement('datalist');
          const optgroups = select.querySelectorAll('optgroup');
          const span = document.createElement('span');
          const options = select.options;
          const parent = select.parentElement;
          const multiple = select.hasAttribute('multiple');
          const onclick = function(e) {
              const disabled = this.hasAttribute('data-disabled');
              select.value = this.dataset.value;
              span.innerText = this.dataset.label;
              if (disabled) return;
              if (multiple) {
                  if (e.shiftKey) {
                      const checked = this.hasAttribute("data-checked");
                      if (checked) {
                          this.removeAttribute("data-checked");
                      } else {
                          this.setAttribute("data-checked", "");
                      };
                  } else {
                      const options = div.querySelectorAll('.option');
                      for (i = 0; i < options.length; i++) {
                          const option = options[i];
                          option.removeAttribute("data-checked");
                      };
                      this.setAttribute("data-checked", "");
                  };
              };
          };
          const onkeyup = function(e) {
              e.preventDefault();
              e.stopPropagation();
              if (e.keyCode === 13) {
                  this.click();
              }
          };
          div.classList.add('select');
          header.classList.add('header');
          div.tabIndex = 1;
          select.tabIndex = -1;
          span.innerText = select.label;
          header.appendChild(span);
          for (attribute of select.attributes) div.dataset[attribute.name] = attribute.value;
          for (i = 0; i < options.length; i++) {
              const option = document.createElement('div');
              const label = document.createElement('div');
              const o = options[i];
              for (attribute of o.attributes) option.dataset[attribute.name] = attribute.value;
              option.classList.add('option');
              label.classList.add('label');
              label.innerText = o.label;
              option.dataset.value = o.value;
              option.dataset.label = o.label;
              option.onclick = onclick;
              option.onkeyup = onkeyup;
              option.tabIndex = i + 1;
              option.appendChild(label);
              datalist.appendChild(option);
          }
          div.appendChild(header);
          for (o of optgroups) {
              const optgroup = document.createElement('div');
              const label = document.createElement('div');
              const options = o.querySelectorAll('option');
              Object.assign(optgroup, o);
              optgroup.classList.add('optgroup');
              label.classList.add('label');
              label.innerText = o.label;
              optgroup.appendChild(label);
              div.appendChild(optgroup);
              for (o of options) {
                  const option = document.createElement('div');
                  const label = document.createElement('div');
                  for (attribute of o.attributes) option.dataset[attribute.name] = attribute.value;
                  option.classList.add('option');
                  label.classList.add('label');
                  label.innerText = o.label;
                  option.tabIndex = i + 1;
                  option.dataset.value = o.value;
                  option.dataset.label = o.label;
                  option.onclick = onclick;
                  option.onkeyup = onkeyup;
                  option.tabIndex = i + 1;
                  option.appendChild(label);
                  optgroup.appendChild(option);
              };
          };
          div.onclick = function(e) {
              e.preventDefault();
          }
          parent.insertBefore(div, select);
          header.appendChild(select);
          div.appendChild(datalist);
          datalist.style.top = header.offsetTop + header.offsetHeight + 'px';
          div.onclick = function(e) {
              if (multiple) {

              } else {
                  const open = this.hasAttribute("data-open");
                  e.stopPropagation();
                  if (open) {
                      this.removeAttribute("data-open");
                  } else {
                      this.setAttribute("data-open", "");
                  }
              }
          };
          div.onkeyup = function(event) {
              event.preventDefault();
              if (event.keyCode === 13) {
                  this.click();
              }
          };
          document.addEventListener('click', function(e) {
              if (div.hasAttribute("data-open")) div.removeAttribute("data-open");
          });
          const width = Math.max(...Array.from(options).map(function(e) {
              span.innerText = e.label;
              return div.offsetWidth;
          }));
          console.log(width)
          div.style.width = width + 'px';
      }
      document.forms[0].onsubmit = function(e) {
          const data = new FormData(this);
          e.preventDefault();
          submit.innerText = JSON.stringify([...data.entries()]);
      }
      </script>
  </body>
</html>
