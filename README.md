# Shinobi Chronicle

A one-to-four-player life simulator. Roll into a village, get the clan your
blood gives you, and live it out across generations.

Play it here once it is hosted: `https://<your-username>.github.io/<repo>/`

---

## What goes in the repository

Keep this exact structure. The names matter.

```
your-repo/
  index.html
  audio/
    menu-song-1.mp3
    menu-song-2.mp3
    menu-song-3.mp3
    menu-song-4.mp3
    menu-song-5.mp3
```

`README.md` is optional and does nothing in the game.

**Uploading through the browser:** drag the whole `audio` folder in at once.
Dragging the five files individually puts them in the root and nothing will play.

---

## Part 1 - GitHub Pages

1. Create a public repository.
2. Upload `index.html` and the `audio` folder, keeping the structure above.
3. **Settings - Pages - Source: Deploy from a branch**, branch `main`,
   folder `/ (root)`. Save.
4. Wait a minute, then open the address.

Music starting on its own means the files are hosted correctly.

Same-screen play works from this point. Online rooms need part 2.

---

## Part 2 - a database for online rooms

Without this, Host and Join are greyed out and the page says why.

Firebase's free tier covers this at no cost.

1. `https://console.firebase.google.com` - **Add project**. Analytics off.
2. **Build - Realtime Database - Create Database**. Any location.
   Choose **Start in test mode**.
3. Copy the URL at the top of the Data tab, for example
   `https://yourproject-default-rtdb.firebaseio.com`
4. Open `index.html` in a text editor, find this line near the top of the
   script and paste your URL between the quotes:

   ```js
   const DB_URL = "";
   ```

5. Save, re-upload, reload the page.

### Test mode expires after 30 days

When rooms stop working, that is why. Go to **Realtime Database - Rules** and
replace them with:

```json
{
  "rules": {
    "rooms": {
      ".read": true,
      ".write": true
    }
  }
}
```

This is deliberately open, because the game has no accounts. Anyone who knows a
five character room code can read that room, so keep real personal details out
of character names.

---

## Playing together

- Everyone opens the same address.
- One person picks the player count and mode, then **Host**, and reads out the
  five character room code.
- Everyone else picks **Join a room** and enters it.
- Once every seat is filled you all drop into the game together.
- Nobody waits for anybody. You all play the same year at the same time and it
  rolls over once everyone has ended theirs.

**If someone closes their tab** they are not locked out. Joining a full room
offers the roster so they can pick their own character back up, and a **Rejoin**
card appears on the title screen of the device that was in the room. If somebody
goes quiet for good, the others get a button to carry the year without them.

---

## If something is wrong

**No music.** Check the `audio` folder uploaded, is lowercase, and sits beside
`index.html`. The browser console will show 404s on the mp3 files.

**Host and Join greyed out.** `DB_URL` is still empty, or the edited file was
not re-uploaded.

**Header says "database unreachable".** Wrong URL, or the Firebase rules
expired. Check the rules first, then look for a trailing slash or typo.

**"That room is already full."** Everyone is in already. If you were one of
them, the roster picker appears instead so you can reclaim your seat.

**Changes not showing.** GitHub Pages caches. Hard reload with Ctrl+Shift+R,
or Cmd+Shift+R on a Mac.

---

## Notes

- The audio is about 23 MB. First load takes a moment on a slow connection,
  then the browser caches it.
- Everyone should be on the same version. After you change anything, have
  everyone hard reload.
- Same-screen saves and the hall of records live in browser storage and stay on
  that one device.
