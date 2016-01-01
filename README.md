## start

- `npm install && npm start`
- go to `http://localhost:8000/`

## Results

### Prompts

- Chrome displays non-blocking prompt after target page is loaded
- Firefox displays non-blocking prompt immediately after submit occures
- Safari displays UI blocking prompt after submitting form renders target page behind it

### Test cases

1. There's one form containing text and password fields. It is submitted using POST method.
Action url differs form page url (`/index.html` and `/`, correspondingly), but the same content
is served
2. Action url differs and content differs, too (`/index1.html` -> `/login.html`)
3. The same as previous, but there're no names in `<input>`s (`/index2.html` -> `/login.html`)
4. The same as previous, but there're two inputs `type="password"` (`/index3.html` -> `/login.html`)
5. The same as previous, but there're three inputs `type="password"` (`/index4.html` -> `/login.html`)
6. The same as previous, but there're 4 inputs `type="password"` (`/index5.html` -> `/login.html`)
7. The same as previous, but there're 4 inputs `type="password"` w/o name and one with name set (`/index6.html` -> `/login.html`). First four have different values
8. As previous, but unnamed passwords have `display:none` style (`/index7.html` -> `/login.html`)
9. As `7`, but unnamed passwords have `position: absolute; left: -9999px;` style (`/index8.html` -> `/login.html`)
10. Just like `2`, but `form` and `input` have `autocomplete="off"` set, also there's `onsubmit` event handler, 
which replaces password field with `hidden` one and resubmits the form with 50ms delay
11. As `10`, but password field is not removed, instead value is set to empty string

### Chrome 47, 49 (Mac)

1. Does not ask to save password
2. Requires password input to be non-empty to prompt
3. Requires password input to be non-empty to prompt
4. Requires one password to be non-empty or both to have the same value to prompt
5. Prompts in case first one is the only non-empty or first and second are non-empty and
have the same value (whether 3d one is empty or not)
6. As previous one
7. Never prompts (assuming unnamed password fields are not changed)
8. CH47 - Never prompts (assuming unnamed password fields are not changed), CH49 (uses "Google Smart Lock") - Prompts when visible password is not empty
9. Never prompts (assuming unnamed password fields are not changed)
10. Never prompts
11. Prompts when password input is non-empty

### FF 42

1. Requires password input to be non-empty to prompt
2. Requires password input to be non-empty to prompt
3. Requires password input to be non-empty to prompt
4. Requires at least one password to be non-empty to prompt, uses **last** one if both are filled
5. As earlier, but does not prompt in case all **three** passwords are different
6. Does not prompt if there're three non-empty different passwords 
7. Never prompts (assuming unnamed password fields are not changed)
8. Never prompts (assuming unnamed password fields are not changed)
9. Never prompts (assuming unnamed password fields are not changed)
10. Prompts when password input is non-empty (value of renamed property is used by
password manager)
11. Prompts when password input is non-empty

### Safari 9

1. Requires password input to be non-empty to prompt
2. Requires password input to be non-empty to prompt
3. Requires password input to be non-empty to prompt
4. Requires at least one password to be non-empty to prompt (which one if both are filled?)
5. Does not prompt in case all passwords are empty or first one is only not empty
6. Never prompts
7. Never prompts (assuming unnamed password fields are not changed)
8. Prompts when visible password is not empty
9. Prompts when visible password is not empty
10. Prompts when password input is non-empty (also it removes second input when going
back)
11. Prompts when password input is non-empty
