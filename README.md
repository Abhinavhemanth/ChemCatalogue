# Chemistry Lab Chemical Catalogue

A simple website that lists all chemicals in the M.Sc. and B.Sc. chemistry laboratories. It shows each chemical's name, formula, hazards, availability and the date it was added. It also has safety procedures (SOPs), a hazard symbol guide and an About section for the college.

The whole site is one file, `index.html`. There is nothing to install or build.


## 1. FILES

    index.html       The complete website (pages, design and logic)
    firestore.rules  Security rules to paste into Firebase (Section 7)
    README.md    This guide


## 2. PUBLISHING ON GITHUB PAGES

1. Sign in to github.com and click New repository.
2. Give it a name (for example `chem-catalogue`) and create it.
3. Click "Add file" > "Upload files", upload `index.html`, `firestore.rules` and `README.md`, then click Commit changes.
4. Open Settings > Pages.
5. Under "Build and deployment", choose Source: "Deploy from a branch".
6. Choose Branch: `main` and Folder: `/ (root)`, then click Save.
7. Wait one to two minutes. Your site will be at:

       https://YOUR-USERNAME.github.io/REPOSITORY-NAME/

To update the site later, open `index.html` in the repository, click the pencil icon (or upload a new copy), and commit. After updating, press Ctrl+Shift+R on the live site so the browser loads the new version.


## 3. PAGES AND WHAT THEY CONTAIN

HOME
- Button to open the chemical catalogue
- About the chemicals (acids and bases, salts, solvents, indicators)
- Standard operating procedures for dangerous chemicals
- Hazard symbols (GHS pictograms) and their meanings
- About section for the college
- Login button at top right

CATALOGUE
- Chemicals listed in alphabetical order, one per row
- Chemical name and formula (subscripts are added automatically)
- Hazard warnings for hazardous chemicals
- Availability: In stock, Low stock or Out of stock, with quantity
- Date added
- Search box (name, formula or hazard) and filters for lab and availability

LOGIN
- Welcome message, username and password box, Log in button


## 4. WHO CAN DO WHAT

Visitors (not logged in)
- View and search the catalogue and read all safety information.

Teachers (logged in)
- Add chemicals, delete chemicals and change availability.

Administrator (HOD)
- Everything teachers can do, plus create and remove teacher logins.


## 5. DAILY USE

ADD A CHEMICAL
1. Log in and open the Catalogue page.
2. Click "+ Add a chemical".
3. Enter the name and formula, choose the lab, enter the quantity and availability, and tick the hazards.
4. Click "Add to catalogue". Today's date is recorded automatically.

Write formulas with plain digits, for example `H2SO4` or `CuSO4·5H2O`. The site turns the numbers into subscripts.

CHANGE AVAILABILITY
- Logged in, use the dropdown in the Availability column of the chemical's row.

DELETE A CHEMICAL
- Logged in, click Delete at the end of the row and confirm.

SEARCH
- Type in the search box. The list narrows as you type. Use the dropdowns to filter by lab or availability.

CREATE A TEACHER LOGIN (administrator only)
1. Log in as administrator.
2. Click the round profile icon at top right.
3. Choose "Manage teacher logins".
4. Enter the teacher's name, a username and a password (at least 6 characters), then click Create login.
5. Give the username and password to the teacher.
6. Use Remove next to a teacher to take away their access.


## 6. CUSTOMISING THE SITE

Open `index.html` in any text editor, or in GitHub with the pencil icon.

COLLEGE DETAILS
Near the top of the script, find the block that starts with `const COLLEGE`. Change the name, department, place, description and contact email. These appear in the About section.

STARTING CHEMICAL LIST
The catalogue starts empty. Log in and add your chemicals, or use "Import catalogue" with a backup file (Section 8).

SAFETY PROCEDURES
Edit the `SOPS` list to change or add procedures. Please have your faculty-in-charge review them so they match your lab's rules.

COLOURS
At the top of the style section, the `:root` block holds the colours (`--ink`, `--teal`, `--amber` and so on). Change these values to restyle the whole site.


## 7. SETTING UP THE ONLINE DATABASE (FIREBASE) - ONE TIME

The catalogue is stored online in Google Firebase (free "Spark" plan, no card needed), so every teacher sees the same live list on any device. Real logins are handled by Firebase, not by the page.

1. Go to console.firebase.google.com, sign in with a Google account and click "Create a project". Turn off Google Analytics.
2. Build > Authentication > Get started > Sign-in method > Email/Password > Enable > Save.
3. Authentication > Users > Add user. For the administrator enter:
   - Email: your admin username converted to an email. The site does this by making the username lowercase, turning symbols into hyphens and adding `@chemcat.app`. So `Admit@Chem` becomes `admit-chem@chemcat.app`.
   - Password: the admin password you want (at least 6 characters).
4. Build > Firestore Database > Create database > choose a location near you (for example asia-south1, Mumbai) > start in production mode.
5. Firestore Database > Rules tab. Delete what is there, paste the full contents of `firestore.rules`, and click Publish.
6. Project settings (gear icon) > General > "Your apps" > click the web icon (</>) > register the app (skip hosting). Copy the four values shown (apiKey, authDomain, projectId, appId).
7. In `index.html`, find `const FIREBASE_CONFIG` and paste those four values in place of `PASTE_HERE`. Commit the file.
8. If login ever fails with a domain error, go to Authentication > Settings > Authorized domains and add `YOUR-USERNAME.github.io`.

Log in with your admin username and password. You will see the profile icon with "Manage teacher logins".

To change the admin username, change `ADMIN_USERNAME` in `index.html`, create that user in Firebase (step 3), and change the admin email inside `firestore.rules` to match, then publish the rules again.

It is normal and safe for the Firebase settings to be visible in a public repository. They only identify your project. The security comes from the rules and logins above.


## 8. BACKUP AND RESTORE

Your data now lives online, so refreshing, clearing the browser or switching computers does not lose it. A backup is still a good habit.

- Backup: profile icon > "Export catalogue (backup)" downloads `chem-catalogue-backup.json`.
- Restore: profile icon > "Import catalogue" and choose the file. Imported chemicals are added to the list (existing ones are not replaced, so do not import the same file twice).
- Teachers: Remove in "Manage teacher logins" takes their access away immediately. Adding the same username again restores access with their old password.


## 9. LIMITATIONS AND SECURITY

- Anyone can view the catalogue. Only logged-in teachers and the administrator can add, change or delete, and this is enforced by Firebase rules, not just by hiding buttons.
- Teacher passwords cannot be reset from the site. If a teacher forgets one, remove them, then ask them to use a new username (or reset the password in the Firebase console under Authentication > Users).
- Use strong passwords, especially for the administrator.
- The free plan has generous daily limits that a college lab will not come close to.


## 10. TROUBLESHOOTING

"Firebase is not set up yet" message
- The `PASTE_HERE` values in `index.html` are still there. Complete Section 7.

Cannot log in
- Check the username spelling and the password. Confirm the user exists in Firebase > Authentication > Users with the converted email (Section 7, step 3).

Login works but adding a chemical fails
- The rules were not published, or the admin email in `firestore.rules` does not match the admin user. Re-check Section 7, step 5.

Catalogue is empty
- Check your internet connection and that the Firestore database was created (step 4). Add chemicals after logging in.

Changes to the site do not show
- Press Ctrl+Shift+R to reload without the old cached copy.

Fonts look different
- The site loads Google Fonts. Without internet it falls back to system fonts.


## 11. SAFETY NOTE

The hazard and SOP information is general guidance for students and staff. Always follow your department's safety rules and the manufacturer's Safety Data Sheet (SDS) for each chemical.
