# BSECE-1B Student Portal

A responsive section portal for class schedules, announcements, tasks, shared learning links, class directory, polls, GPA planning, and introductory ECE calculators.

## Included features

### Section workspace
- Student sign-in and account creation (when Supabase is configured)
- Officer-only content management
- Dashboard, weekly class schedule, announcements, deadlines, resource links
- Class directory, class officers, and polls
- Browser-local preview mode for testing the UI

### Engineering toolkit
- Ohm's law (V, I, R, P)
- Series and parallel resistor equivalents
- Voltage divider
- LED current-limiting resistor estimate
- Capacitive and inductive reactance
- RC time constant and approximate 5τ settling time
- Frequency-to-wavelength conversion
- Four-band resistor color code decoder
- Binary / octal / decimal / hexadecimal conversion
- Logic gate evaluator (AND, OR, XOR, NAND, NOR, XNOR, NOT)
- Electrical and metric unit conversions
- Weighted GPA and target GPA calculators

## Publish on GitHub Pages

1. Create a new GitHub repository, for example `bsece-1b-portal`.
2. Upload **all files and folders** from this package. `index.html` must be in the repository root.
3. Open **Settings → Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**.
5. Select branch `main` and folder `/ (root)`, then Save.
6. Wait for GitHub Pages to publish. Your site URL will look like:
   `https://YOUR-USERNAME.github.io/bsece-1b-portal/`

GitHub Pages only hosts the static website. It does not provide user accounts or a shared database.

## Enable shared accounts and cloud data with Supabase

Without Supabase, Preview mode uses browser local storage. Each classmate sees only data stored in their own browser. To share data securely, configure Supabase:

1. Create a Supabase project at https://supabase.com/.
2. Open **SQL Editor** in your Supabase dashboard.
3. Paste and run `supabase/schema.sql`.
4. In **Project Settings → API**, copy the Project URL and the `anon` / publishable key.
5. Edit `config.js`:
   ```js
   window.PORTAL_CONFIG = {
     supabaseUrl: "https://YOUR-PROJECT.supabase.co",
     supabaseAnonKey: "YOUR_SUPABASE_ANON_OR_PUBLISHABLE_KEY"
   };
   ```
6. Commit and push the change to GitHub. The site will reload with cloud authentication.
7. Students can create accounts using their email addresses. In Supabase Auth settings, configure email confirmation and allowed redirect URLs for your GitHub Pages URL.
8. After an intended officer account has been created, run this in Supabase SQL Editor, replacing the email:
   ```sql
   update public.profiles
   set role = 'officer'
   where email = 'officer-email@example.com';
   ```
9. Ask students to sign in. Authenticated students can read shared section data. Officers can create, update, and delete shared records.

### Security notes
- The Supabase anon/publishable key is designed to be used in a public frontend **only with Row Level Security (RLS) enabled** and correct policies. The included schema sets up basic policies.
- Never put a Supabase `service_role` key in `config.js`, GitHub, or browser code.
- Restrict signups to your section where possible. For a small private class portal, consider manually inviting students or using Supabase Auth allowlists/Edge Functions for stronger membership control.
- The starter app uses basic officer role checks. For a high-stakes production system, have a developer review the policies and implement stronger role management, audit logs, rate limits, and verified class membership.
- The starter polls are a simple shared counter, not a privacy-preserving election system. Do not use them for sensitive or official votes.
- The directory should contain only information classmates explicitly agree to share. Do not store grades, student numbers, addresses, or sensitive records in public tables.

## Local preview

Open `index.html` in a modern browser. Use **Continue in preview mode**. Preview mode is intended to test the interface; local data is not synchronized across devices.

## Customize

- Edit `config.js` to connect Supabase.
- Edit `app.js` to add tools or change modules.
- Edit `styles.css` to customize the visual design.
- Update sample schedule and resource data in `app.js` before presenting the site.

## Important limitations

This is a starter project, not an official Bulacan State University system. Calculators are educational aids and may omit real-world component tolerances, loading effects, parasitics, thermal limits, or institutional grading rules. Verify results independently.
