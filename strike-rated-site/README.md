# Strike Rated website + Scout agent

A one-page site with Scout, our AI intake agent, built in. Free to run:
Netlify's free plan hosts the site and Scout's function, Google's free Gemini API powers Scout,
and Netlify Forms (free, 100 submissions a month) saves every finished conversation and pilot sign-up.

## Files
- `index.html`: the website, including the Scout chat and the "Join the list" form
- `netlify/functions/scout.mjs`: Scout's brain (instructions, fact sheet, limits). Edit the fact sheet here.
- `netlify.toml`: tells Netlify where things are

## Set up (about 20 minutes)

1. **Get a free Gemini API key.** Go to https://aistudio.google.com/apikey, sign in with a Google account, click Create API key, copy it. Never paste it into `index.html` or share it.
2. **Fill in your contact.** In `index.html`, replace `[team email]` in the footer with your team's email.
3. **Put the folder on GitHub.** Create a new repository on github.com, click "uploading an existing file", and drag in everything in this folder, including the `netlify` folder. Commit.
   (Netlify's drag-and-drop deploy may skip Scout's function, so use GitHub.)
4. **Deploy on Netlify.** app.netlify.com → Add new site → Import an existing project → GitHub → pick the repo. Leave the build command empty, publish directory `.` → Deploy.
5. **Add the settings.** Site configuration → Environment variables → add:
   - `GEMINI_API_KEY` = your key (required)
   - `TEAM_EMAIL` = your team email
   - `SCOUT_TIMES` = your three interview times separated by `|`, e.g. `Mon Nov 2, 4:30 PM MT|Tue Nov 3, 4:30 PM MT|Wed Nov 4, 4:30 PM MT` (if you skip this, Scout offers 4:30 PM MT on the next three weekdays)
6. **Turn on forms.** Site configuration → Forms → Enable form detection. Then Deploys → Trigger deploy → Deploy site (forms are only detected on a deploy after this is on).
7. **Get notified.** Forms → Form notifications → Add notification → Email, for `scout-signups` and `pilot-waitlist`.

## Where the conversations show up
Netlify → your site → Forms → `scout-signups`. Each entry has the person's answers, Scout's fit call
(riley_fit / parent_or_trainer / low_fit), any handoff reason, the full transcript, and a draft follow-up email.
A founder reads the draft, edits it, and sends it from their own email. The first ten go out only after a founder reads each one.
Entries marked `left early` are people who gave an email and then closed the page.

## Changing Scout
- Times or contact: change the environment variables and redeploy.
- Facts: edit the fact sheet (`KB`) at the top of `scout.mjs`. Whatever is not in it, Scout should say it doesn't know.
- Rules: edit `systemPrompt` in `scout.mjs`. Log each change in your prompt log.

## If something breaks
- "Scout isn't set up yet": `GEMINI_API_KEY` is missing. Add it and redeploy.
- Scout replies with an AI-service error: check Netlify → Logs → Functions → scout. If the model name was retired, set `GEMINI_MODEL` to a current free model listed in Google AI Studio.
- Nothing in Forms: form detection was off during the deploy. Turn it on and redeploy.
