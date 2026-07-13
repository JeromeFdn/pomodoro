Focus Timer (Pomodoro)
======================

A single-page focus timer: 25 minutes of work, then a 5 minute break, with
session tracking and a break-audio loop. Everything runs in the browser from one
HTML file. No build step, no server, no dependencies.


CONTENTS
--------
  pomodoro.html    The whole app: markup, styles, and logic in one file.
  break-audio.mp3  The loop that plays during breaks. Swap it for any mp3.
  README.txt       This file.


RUNNING IT
----------
The simplest path is GitHub Pages. Push these files to a repo, enable Pages
(Settings > Pages > deploy from the main branch), and open the URL it gives you,
for example https://jeromefdn.github.io/<repo>/pomodoro.html.

You can also open pomodoro.html straight from disk, but two things degrade that
way: desktop notifications may not prompt or persist (a local file has no stable
origin for Chrome to remember), and browser extensions are more likely to get in
the way. Hosting on Pages avoids both.


HOW IT WORKS
------------
Click Start focus to begin a 25 minute block. At zero it chimes, and if you’ve
granted notification permission and the tab is in the background, it also fires a
desktop notification. You acknowledge to start the 5 minute break, during which
break-audio.mp3 loops. When the break ends it rolls straight into another focus
block.

The countdown reads from the system clock rather than counting seconds, so it
stays accurate even when the tab is backgrounded and the browser throttles its
timers. A small background worker keeps it ticking so the end-of-focus chime
fires close to on time even while you’re looking at another tab.

Every completed focus block is recorded with a timestamp and whether it fell
inside your set work hours. The lower panel shows sessions today, sessions during
work hours, and a rolling 7 day count, plus a list of recent sessions. Start new
day returns everything to the READY state from wherever you are, without erasing
your recorded history.


BREAK AUDIO
-----------
The break loops whatever mp3 is named break-audio.mp3 in this folder. To change
the sound, replace that file (keep the name, or edit the <source> tag in
pomodoro.html). Pick something that loops cleanly, since a short clip restarts
mid-break and a rough seam is audible.

If the file is missing or the browser refuses to play it, the app falls back to a
quiet synthesized ambient hum so breaks are never silent. If you hear the hum
instead of your track, open the browser console (View > Developer > JavaScript
Console in Chrome) for the reason, usually a wrong filename or path.


NOTIFICATIONS
-------------
The first time you click Start focus, Chrome asks to allow notifications. Grant it
once on the hosted URL and Chrome remembers it for that origin. You can review or
change it at the tune icon in the address bar under Site settings > Notifications,
or at chrome://settings/content/notifications.

On a Mac there’s a second layer: even with Chrome’s permission granted, macOS has
to allow Chrome to post notifications. Check that System Settings > Notifications
> Google Chrome is set to Allow, and that a Focus mode or Do Not Disturb isn’t
quietly holding them back. That’s the usual reason a notification never appears
despite the browser saying yes.


DATA AND PRIVACY
----------------
Sessions and settings are stored locally in your browser under the key
pomodoro-data. Nothing leaves your machine, and there’s no account or server.
Clearing the site’s browser data, or switching to a different browser or profile,
starts you from an empty history.


CUSTOMIZING
-----------
The two durations live near the top of the script in pomodoro.html:

  const FOCUS_SECONDS = 25 * 60;
  const REST_SECONDS  = 5 * 60;

Change those numbers to adjust the block and break lengths. Work hours (default
9 AM to 5 PM, Monday to Friday) are set inside the app under Work hours and are
used only to tag sessions, not to drive the timer.


TROUBLESHOOTING
---------------
Timer looks frozen when the tab is in the background: it should recover the moment
you refocus the tab. If it truly stops, your browser may be suspending the tab
outright, which some battery-saver settings do.

No audio during breaks: confirm break-audio.mp3 sits next to pomodoro.html and the
name matches exactly, then check the console for the specific error. The
synthesized hum is the signal that the file did not load.

No desktop notification: check both the Chrome site setting and, on a Mac, System
Settings > Notifications for Chrome plus any active Focus mode.


BROWSER SUPPORT
---------------
Built for current Chrome, and works in current Safari, Firefox, and Edge. mp3
plays everywhere; add an optional break-audio.ogg source only if you care about
older Firefox.
