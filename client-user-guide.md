# A&N Style Website — Client User Guide

**Your website, explained in plain English.**  
No technical knowledge needed to follow this guide.

---

## Table of Contents

1. What Your Website Does
2. How to Open and View Your Website
3. How to Go Live (Publish Your Website)
4. The Booking Form — How It Works
5. How to Update Your Prices
6. How to Change Your Opening Hours
7. How to Update Your Team Members
8. How to Swap in Real Photos
9. How to Change Your Phone or WhatsApp Number
10. The Fresha Booking Widget
11. Google Reviews — How to Add New Ones
12. The Video Showreel
13. The Theme Toggle (Dark / Light Mode)
14. What Happens When a Customer Books
15. Getting Help

---

## 1. What Your Website Does

Your website (`index.html`) is a single file that contains your entire A&N Style website. It includes:

- A homepage hero with your headline and booking buttons
- A full services and pricing menu (19 services)
- Staff profile cards for Nabi, Alex and Sam
- A photo gallery with a lightbox viewer
- A video showreel section
- Real customer reviews from Google
- A live booking availability placeholder (Fresha)
- An opening hours table
- A booking/contact form (connected to your email via Formspree)
- Your location on Google Maps
- A WhatsApp floating button
- A dark/light mode switch

---

## 2. How to Open and View Your Website

**On your computer (for previewing before going live):**

1. Find the file called `index.html` on your computer
2. Double-click it — it will open in your web browser (Chrome, Safari, Firefox, Edge)
3. You will see your website exactly as customers will see it

> ⚠️ The Google Maps section may not display when opened as a local file. This is normal — it will work correctly once the site is published online.

---

## 3. How to Go Live (Publish Your Website)

Your website needs to be "hosted" online so customers can visit it. We recommend **Netlify** — it's free and takes under 5 minutes.

**Step-by-step:**

1. Go to **netlify.com** and create a free account
2. Click **"Add new site"** → **"Deploy manually"**
3. Open the folder on your computer that contains `index.html` and the `images_and_videos` folder
4. **Drag the entire folder** into the Netlify drop zone
5. Netlify gives you a web address immediately (e.g. `an-style.netlify.app`)
6. To use your own custom address (e.g. `anstyle.co.uk`), click **"Domain settings"** and follow the steps

> 📌 **Important:** Always upload the `images_and_videos` folder together with `index.html`. If the folder is missing, your local photos and video will not appear on the live site.

**Every time you make a change to the website**, repeat steps 2–4 to update the live site.

---

## 4. The Booking Form — How It Works

Your booking form (in the "Book Your Appointment" section) is connected to **Formspree**, which emails you every time a customer fills it in.

**What customers fill in:**
- Full name
- Phone number
- Which service they want
- Which stylist they prefer
- Their preferred date and time

**What happens next:**
- The customer sees a confirmation message on screen
- You receive an email at the address registered with your Formspree account
- You then call the customer to confirm

**To see all your bookings:**
- Log in at **formspree.io**
- Go to your form (`mgojaorz`) to view a log of all submissions

> 📌 The free Formspree plan allows up to **50 submissions per month**. If you receive more than that, you can upgrade your plan at formspree.io for a small monthly fee.

---

## 5. How to Update Your Prices

To change a price, you need to edit the `index.html` file in a text editor.

**Recommended free text editors:**
- **Notepad++** (Windows) — download at notepad-plus-plus.org
- **TextEdit** (Mac) — comes pre-installed; set it to plain text mode
- **VS Code** (any platform) — download at code.visualstudio.com (best option)

**Steps:**

1. Open `index.html` in your text editor
2. Press **Ctrl+F** (Windows) or **Cmd+F** (Mac) to open the search box
3. Type the service name, e.g. `Skin Fade`
4. Find the price next to it — it will look like: `£32`
5. Change the number to the new price
6. **Also search** for `Skin Fade — £32` (with the price) — this appears in the booking form dropdown. Update this too so the dropdown matches
7. Save the file (`Ctrl+S` or `Cmd+S`)
8. Re-upload to Netlify to make the change live

---

## 6. How to Change Your Opening Hours

Hours appear in **two places** in the website — you need to update both.

**Place 1 — The full hours table** (in the "Opening Hours" section):
Search for `9:00 AM – 7:30 PM` to find the rows. Change the times as needed for each day.

**Place 2 — The compact hours summary** (in the footer):
Search for `Mon – Fri` — the condensed version is just below. Update these to match.

**Example — changing Saturday hours from 9–6 to 9–5:**

Find: `9:00 AM – 6:00 PM` (in the Saturday row)  
Change to: `9:00 AM – 5:00 PM`

Do this in both the main hours table and the footer summary.

---

## 7. How to Update Your Team Members

Each team member has a profile card in the "Meet the Team" section. Each card contains:
- An initial avatar (the large letter)
- Name
- Job title
- Short description

**To change a name or description:**

1. Open `index.html` in your text editor
2. Search for the team member's name (e.g. `Nabi`)
3. Find the card — it will look like:
   ```
   Nabi
   Master Barber & Stylist
   Specialist in sharp skin fades...
   ```
4. Edit the text directly
5. Save and re-upload

**To add a new team member:**
This requires copying one of the existing staff card blocks and editing it. Ask your developer to do this, or refer to the Technical Documentation.

---

## 8. How to Swap in Real Photos

Your gallery currently uses a mix of real photos (from your `images_and_videos` folder) and placeholder images from the internet (Unsplash). To replace a placeholder with a real photo:

1. Save your photo into the `images_and_videos` folder on your computer
2. Give it a simple filename with no spaces, e.g. `skinFade.jpg`
3. Open `index.html` in your text editor
4. Search for the caption of the gallery slot you want to replace, e.g. `Gents Cut`
5. Find the `<img src="...">` tag just above or below it
6. Replace the URL with: `./images_and_videos/skinFade.jpg`
7. Save and re-upload the whole folder (including the new image) to Netlify

> 📸 **Best photo sizes:** At least 800px wide. JPG or WebP format gives the best quality-to-file-size balance. Avoid very large files (over 3MB) as they slow the page down.

---

## 9. How to Change Your Phone or WhatsApp Number

**Phone number** appears in the footer. To update it:
1. Search for `442088745322` in the file
2. Replace all occurrences with your new number in the same format (no spaces, no `+`)
3. Also update the displayed text `+44 20 8874 5322` to match the new number

**WhatsApp number** appears on the floating green button and in the footer. To update:
1. Search for `447836445764` in the file
2. Replace all occurrences with your new number (no spaces, no `+`)

> 📌 The number format used in the links is international format without the `+` sign. For a UK number `07700 900 123`, write it as `447700900123`.

---

## 10. The Fresha Booking Widget

The "Book a Real-Time Slot" section currently shows a **preview/mock** of what a live booking calendar looks like. It is not yet connected to real availability.

**To activate real-time booking:**

1. Create a free business account at **fresha.com**
2. Set up your services, team and availability in the Fresha dashboard
3. Go to **Dashboard → Widgets → Online Booking**
4. Copy the embed code Fresha gives you
5. Send the embed code to your developer — they will replace the mock calendar in the website with the real one

Once connected, customers will be able to see live availability and book instantly without needing to call.

---

## 11. Google Reviews — How to Add New Ones

Your five reviews are hardcoded into the website — they do not automatically pull from Google. When you receive new reviews you're proud of, you can add them manually.

**To add a new review:**

1. Copy one of the existing review card blocks in `index.html`
2. Replace the reviewer's name, review text, and timeframe
3. Change the initial letter in the avatar circle to match the reviewer's first initial
4. Save and re-upload

**To encourage customers to leave reviews:**
The "Leave a Google Review" button in the reviews section links directly to your Google Maps profile. Share this section of your website with satisfied customers.

---

## 12. The Video Showreel

Your video (`A&N Style.mp4`) plays directly on the website from the `images_and_videos` folder.

**To replace the video with a newer one:**
1. Export your new video as an MP4 file
2. Name it `A&N Style.mp4` (same name as the original) and place it in `images_and_videos`
3. Re-upload everything to Netlify

**Or to use a differently named file:**
1. Save the video with its new name in `images_and_videos`
2. Open `index.html` and search for `A&N Style.mp4`
3. Replace the filename with your new file's name
4. Re-upload

> ⚠️ **Note about YouTube:** An earlier version of the website tried to embed a YouTube video, but it showed an error because embedding was disabled on that video. The current version uses a local MP4 file, which always works reliably. If you want to re-enable YouTube embedding, go to YouTube Studio → Content → select the video → Details → "Allow embedding" → Save, then let your developer know.

---

## 13. The Theme Toggle (Dark / Light Mode)

Your website has both a dark theme and a light theme. Visitors can switch between them using the small toggle switch in the navigation bar (🌙 / ☀️).

**The website remembers which theme each visitor chose** — if they switch to light mode and come back later, it will still be in light mode.

**The default is dark mode.** If you prefer light mode as the default, ask your developer to change the opening `<html>` tag from `data-theme="dark"` to `data-theme="light"`.

---

## 14. What Happens When a Customer Books

Here is the full customer journey from visiting your website to you receiving their details:

1. Customer visits your website
2. They browse services, reviews, and the team
3. They click **"Book Online"** or **"Book Now"** — this scrolls them to the booking form
4. They fill in their name, phone, service, stylist preference, and date/time
5. They click **"Request Appointment"**
6. They see a green success message: *"Request Received!"*
7. **You receive an email** from Formspree with all their details
8. You call them back to confirm the appointment

> 💡 **Tip:** Check your Formspree email regularly, or set up email forwarding so booking notifications go straight to your phone.

---

## 15. Getting Help

**For content changes** (prices, hours, text, photos):
This guide covers everything you need. Use a text editor and follow the steps above.

**For structural changes** (adding new sections, new team members, changing layout):
These require a developer. Refer them to the **Technical Documentation** which accompanies this guide.

**For Formspree issues** (not receiving emails, form errors):
Log in at **formspree.io** and check your form's settings and submission log.

**For Netlify issues** (site not updating, domain problems):
Log in at **netlify.com** and check your site's deploy log for errors.

**For Fresha integration:**
Visit **fresha.com/business** — their support team can walk you through setting up the booking widget.

---

*This guide was written specifically for the A&N Style website. Keep it alongside your website files for future reference.*
