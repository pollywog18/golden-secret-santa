# Golden Secret Santa — shared-device private-code edition

This is an updated mobile-friendly black-and-gold website prototype. It is **not a live draw service** yet; it does not generate real assignments or issue working private codes.

## Shared-device participant experience
- Organiser enters the participant list, gift budget, and exchange date.
- Organiser runs one draw only. The server locks the assignments permanently.
- The server generates a separate cryptographically random access code for each participant.
- Organiser sends each code individually through WhatsApp (never post codes in the group).
- Participants can take turns using the same phone, tablet, or computer.
- Each code reveals only the matching participant's recipient.
- After viewing, participant taps "Hide my result", which clears the visible result and code field before handing over the device.
- No account or separate device is required for each participant.

## Security requirements before real use
A secure backend (for example Supabase with server-side functions) is required:
1. Store exchange details and participant records server-side.
2. Generate a random derangement exactly once, ensuring nobody is assigned themselves.
3. Enforce a permanent locked state in the database. Reject every later draw, reset, delete, or assignment-update request.
4. Generate long, random, unguessable codes per participant. Store only a secure hash of each code.
5. Code redemption must return only that participant's own recipient name (and, if enabled, that recipient's wishlist).
6. Rate-limit code attempts, support organiser revocation/reissue only if the policy allows it before draw lock, and never expose the complete assignment list to the browser.
7. Use HTTPS. Do not store assignments or codes in browser local storage, and do not place recipient names in URLs.

## Publishing a preview
1. Create a GitHub repository.
2. Upload `index.html`.
3. Open **Settings → Pages**.
4. Choose **Deploy from a branch**, branch `main`, folder `/ (root)`, and save.
5. Share the resulting GitHub Pages URL as a design preview only until the backend is implemented and tested.

## WhatsApp workflow after backend launch
1. Organiser creates the exchange and checks names, budget, and date.
2. Organiser performs the single draw. Server commits and permanently locks it.
3. Server generates one unique code per participant.
4. Organiser privately sends each person their own code.
5. Family members take turns on the shared device, enter their code, view their match, and tap **Hide my result** before passing the device on.

## Privacy reminder
A shared device can show browser history, screenshots, notifications, clipboard contents, or cached pages. Ask participants to view privately, avoid screenshots, and close or hide the result before passing the device on. The backend should expire sessions promptly and never cache private results.
