# Automatically liking events in your personal Strava feed

1. Navigate to `https://www.strava.com/dashboard/following/100`. Replace `100` with the amount of activities to like.
2. Open a console in developer tools (in Firefox `CTRL+SHIFT+I`) 
3. Run `document.querySelectorAll('[data-testid="kudos_button"]').forEach(function(node) {node.click()});`
4. Now the last 100 activities on your feed should be given kudos.
