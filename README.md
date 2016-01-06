## start

- `npm install && npm start`
- go to `http://localhost:8000/`

## Results

### Prompts

- Chrome displays non-blocking prompt after target page is loaded
- Firefox displays non-blocking prompt immediately after submit occures
- Safari displays UI blocking prompt after submitting form renders target page behind it

### Test cases

0. There's one form containing text and password fields. It is submitted using POST method.
Action url differs form page url (`/index.html` and `/`, correspondingly), but the same content
is served
1. Action url differs and content differs, too (`/index1.html` -> `/login.html`)
2. The same as previous, but there're no names in `<input>`s (`/index2.html` -> `/login.html`)
3. The same as previous, but there're two inputs `type="password"` (`/index3.html` -> `/login.html`)
4. The same as previous, but there're three inputs `type="password"` (`/index4.html` -> `/login.html`)
5. The same as previous, but there're 4 inputs `type="password"` (`/index5.html` -> `/login.html`)
6. The same as previous, but there're 4 inputs `type="password"` w/o name and one with name set (`/index6.html` -> `/login.html`). First four have different values
7. As previous, but unnamed passwords have `display:none` style (`/index7.html` -> `/login.html`)
8. As `7`, but unnamed passwords have `position: absolute; left: -9999px;` style (`/index8.html` -> `/login.html`)
9. Just like `2`, but `form` and `input` have `autocomplete="off"` set, also there's `onsubmit` event handler, 
which replaces password field with `hidden` one and resubmits the form with 50ms delay
10. As `10`, but password field is not removed, instead value is set to empty string

#### Chrome 47, 49 (Mac)

0. Does not ask to save password
1. Requires password input to be non-empty to prompt
2. Requires password input to be non-empty to prompt
3. Requires one password to be non-empty or both to have the same value to prompt
4. Prompts in case first one is the only non-empty or first and second are non-empty and
have the same value (whether 3d one is empty or not)
5. As previous one
6. Never prompts (assuming unnamed password fields are not changed)
7. CH47 - Never prompts (assuming unnamed password fields are not changed), CH49 (uses "Google Smart Lock") - Prompts when visible password is not empty
8. Never prompts (assuming unnamed password fields are not changed)
9. Never prompts
10. Prompts when password input is non-empty

#### FF 42

0. Requires password input to be non-empty to prompt
1. Requires password input to be non-empty to prompt
2. Requires password input to be non-empty to prompt
3. Requires at least one password to be non-empty to prompt, uses **last** one if both are filled
4. As earlier, but does not prompt in case all **three** passwords are different
5. Does not prompt if there're three non-empty different passwords 
6. Never prompts (assuming unnamed password fields are not changed)
7. Never prompts (assuming unnamed password fields are not changed)
8. Never prompts (assuming unnamed password fields are not changed)
9. Prompts when password input is non-empty (value of renamed property is used by
password manager)
10. Prompts when password input is non-empty

#### Safari 9

0. Requires password input to be non-empty to prompt
1. Requires password input to be non-empty to prompt
2. Requires password input to be non-empty to prompt
3. Requires at least one password to be non-empty to prompt (which one if both are filled?)
4. Does not prompt in case all passwords are empty or first one is only not empty
5. Never prompts
6. Never prompts (assuming unnamed password fields are not changed)
7. Prompts when visible password is not empty
8. Prompts when visible password is not empty
9. Prompts when password input is non-empty (also it removes second input when going
back)
10. Prompts when password input is non-empty

### Password and login were saved, autofill

All the pages were visited again after accepting password manager's invitation to store
login data submitted from `/` page

#### FF 

0.-2., 9., 10. +, suggests to update password
3. autofills first password input, suggests to update whenever first value is changed or
second added
4. autofills first password input, suggests to update login data if first walue is
changeg, second or third added, or both second and third are added and the same
5.-8. `-`

#### Chrome 47, 49
0.-2., 9., 10. +, does not prompt
3.-8. `-`

#### Safari 9

Does not autofill on its own: password manager popup might be displayed after clicking
key icon which appears in focused password field.


### add autocomplete="off"

All the pages were copied to files with `_off` suffix (e.g. `index1.html` to
`index1_off.html`) and `autocomplete="off"` string was added to the forms and every password field.

Results are completely indentical to those in previous series w/o `autofill="off"`

