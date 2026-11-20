# Pixel and Tech Studio website: to-do list

Last updated: 20 November 2026

`[x]` means done, `[ ]` means still to do. Items marked **(you)** need something from the team, because they depend on accounts, files or decisions I can't make for you.

---

## 1. Done so far

- [x] Site design, colours and pixel drawing pad
- [x] Loading animation (1.3 seconds) and page-change animation
- [x] Pages: Home, Games, Services, Software, About, Careers, Contact
- [x] Playable Pixel Catch mini game with a scoreboard on each visitor's device
- [x] A page for every game, app and tool, with a media carousel, price, buy and wishlist buttons
- [x] AI call handling demo
- [x] Careers section (not hiring yet, collect contact details)
- [x] Newsletter signup form (opens the visitor's email app)
- [x] Press kit page and privacy policy page (first draft)
- [x] Discord buttons (waiting for the invite link)
- [x] Light and dark mode
- [x] Site saved as `index.html` ready for GitHub

---

## 2. Must do before launch

### Real content **(you)**
- [ ] Replace the six sample software entries with your real games, apps and tools
- [ ] Write a real description and a "What you can do" list for each one
- [ ] Decide prices (or "Free") for each item
- [ ] Add real download links, or store links (Steam, itch.io, App Store, Google Play)
- [ ] Add wishlist links for games that aren't out yet
- [ ] Take screenshots (at least 3 per game) and record a short trailer if you have one
- [ ] Add your real Discord invite link, set to never expire
- [ ] Check every piece of wording I wrote for the About, Services, FAQ and Careers sections, and change anything that doesn't sound like you
- [ ] Decide whether the "How a project works" steps match how you really work
- [ ] Replace the made-up plumber example in the AI call demo, or keep it

### Contact and email **(you)**
- [ ] Set up a contact form service so messages arrive without needing an email app (Formspree, Web3Forms or Netlify Forms)
- [ ] Set up a real newsletter service and replace the signup form (Buttondown, MailerLite or Mailchimp)
- [ ] Consider a business email address on your own domain, such as hello@yourdomain.co.uk
- [ ] Test every form by sending a message to yourself

### Legal **(you)**
- [ ] Have someone read the privacy policy and check it matches what you really do
- [ ] Check whether you need to pay the data protection fee to the ICO (ico.org.uk)
- [ ] Update the privacy policy once you add a mailing list service, analytics or a contact form service
- [ ] Add terms of use or a short licence for downloads, if you want one
- [ ] Add age ratings or content notes to any game that needs them
- [ ] Check you have the right to use any music, art or fonts in your games

### Hosting and domain **(you)**
- [ ] Create a GitHub account and repository, and upload `index.html`
- [ ] Turn on GitHub Pages (Settings, then Pages)
- [ ] Buy a domain, for example pixelandtech.co.uk
- [ ] Point the domain at the site and turn on HTTPS
- [ ] Update the links in your press kit and social accounts once the domain is live

---

## 3. Should do soon after launch

### Search and sharing
- [ ] Add a favicon (the little icon in the browser tab) for the GitHub version
- [ ] Add a preview image so links look good when shared on WhatsApp, Discord and social media
- [ ] Add a page title and description for each page, so Google shows the right text
- [ ] Add the site to Google Search Console
- [ ] Create a Google Business Profile for the Newcastle upon Tyne studio

### Quality checks
- [ ] Test on an iPhone, an Android phone, a tablet and a laptop
- [ ] Test in Chrome, Safari, Firefox and Edge
- [ ] Check the site works with the keyboard only
- [ ] Run a free speed and accessibility check (Google Lighthouse in Chrome)
- [ ] Ask two or three friends or teammates to use the site and tell you what confused them

### Press kit
- [ ] Add game screenshots and key art to the press kit
- [ ] Add a short "founded in" date and team names, if you want them public
- [ ] Add links to your social accounts

---

## 4. Nice to have

- [ ] Social media links (Instagram, TikTok, YouTube, X, itch.io)
- [ ] Examples of client work or case studies
- [ ] Testimonials from early clients or players
- [ ] Pricing or "from £X" packages for web design and AI call handling
- [ ] A blog or news page for release notes and behind-the-scenes posts
- [ ] A shared leaderboard for Pixel Catch (needs a database service such as Firebase or Supabase)
- [ ] Privacy-friendly visitor stats (Plausible or Cloudflare Web Analytics), and update the privacy policy if you add them
- [ ] A "Watch the full trailer" button that opens YouTube
- [ ] A cookie notice, only if you add tracking or advertising
- [ ] Team page with photos and short bios

---

## 5. How the site is built (for whoever updates it)

- The whole site is one file, `index.html`. No build step is needed.
- Your software list is the `APPS` list near the top of the script. Each entry has its name, type, price, links and carousel media.
- Your Discord invite link goes in `DISCORD_URL`, just above `wireDiscord`.
- The press kit download buttons only work on the Claude-hosted version of the site. On GitHub they're hidden.
- To change anything, ask Claude for an updated `index.html`, then upload it to the GitHub repo again.
