---
layout: post_default
title:  "Designing HTML emails"
date:   2017-11-16 20:49:00 +4:00:00
categories: ["blog", "programming"]
author: "alicja"
published: true
---
It's 2017 and it seems that plain text emails are just boring for most users.
Communicating without the emoji, GIFs or images seems rather stiff and your
brand is better than that! It's approachable and fun, or sleek and high-end, or
in some other way remarkable. No plain text email will do.

The world of HTML email is the Wild Wild West of the World Wide Web. It's
basically writing websites like it's 2004. Forget flexible user interfaces or
interactive elements  - it's built on tables upon tables, with no JavaScript
and poor CSS support. If you want your design not to break on major clients,
you need to carefully place and think through each element, keeping in mind all
the various ways that your design could break.

### How is email displayed?
The email is sent from a server to an email address that is hosted on another
server. This server then shares the email with various email clients (e.g. the
Mail app on iOS), that decide how to display the content. The server and/or
email client can strip away some or even all elements that are not plain text -
specific or all HTML tags, styles, images, links - leaving the user with the
end content, that is then being displayed.

The main difference between an email and a website is that you can't know
what email client the user is going to choose and so you can't send a specific
version to each user - you send the same email to everyone. You can't detect
beforehand if it's Firefox or Chrome, Gmail or Outlook, you can't swap images
for smaller versions on mobile. You send an email and it sits in your client's
inbox - as soon as they get it, you have no control over how it's going to be
displayed and can't adjust accordingly.

### Understand your users
If you ever went through building a static website or a web app, figuring out
how to support the features you want in most browsers was hard enough. Nobody
expects every web app to work perfectly on every setup - that would take insane
amounts of work and give very low marginal gains. That's why web developers are
telling you they're going to support the modern browsers like Chrome or Firefox
and no-one in their right mind wants to support older Internet Explorers*.

Testing with tools like [Litmus](https://litmus.com/) or [Email on
Acid](https://www.emailonacid.com/) will help you get a better grasp of how
your design works in most popular email clients - but there can still be issues
with clients that are not being tested by those tools. It is impractical to
expect the email to work in **all** clients - that's why it's good to research
how your clients are using email.

1. What [webmail
providers](https://en.wikipedia.org/wiki/Comparison_of_webmail_providers)
and/or [email
clients](https://en.wikipedia.org/wiki/Comparison_of_email_clients) are they
using? Which of those give you most marginal gains and you need to support
them? Consider having a cutoff point for the share of various software in your
client base - adding support for additional clients takes time and might also
cost you some features, maybe it's not worth doing for 0.01% of your clients.

2. How are they accessing their inbox? Do they read emails on their desktop or
their phones? Decide if you want to design mobile- or desktop-first.

3. What email campaigns worked best for the measures that are important to you,
e.g. how many of your users click links in emails? Think about the things you
want to include in your email - you're taking the time to design it, so make
sure you're not spamming your users with irrelevant content.

### Focus on the content
There are many reasons your users will only read plain text emails - some of
them do it by choice, some of them by necessity, e.g. if they are visually
impaired, some simply use email clients that only support plain text. That's
one thing every email client does and they do it pretty well.

Before you design your email, take a moment to focus on the message - why are
you sending an email to your users? If you were to strip away all the design,
what should be included in it? How should you divide it into paragraphs and
make sure the users get your message?

Assuming that your users will always be able to read the HTML version of your
email might be overly optimistic. Don't assume - assure that your message
always gets through and use plain text as your main tool.

There are also things you should consider including in your email, to improve
user experience no matter the client:
- subject - clear and concise, usually under 50 characters
- professional sender address and name - preferably with your company name
- pre-header text - some email clients present it in the inbox as a preview of
an email
- footer - preferably with unsubscribe options
- link to a web-hosted version of the email

### Add the design gradually
You might want to include more than just plain text in your message - your logo
at the top, an image or a GIF that would make it more appealing, a nice button
to encourage users to click the link.

Keep the design simple. Add it in gradually and make sure you always have
fallback options, e.g.:

- for every image have a fallback text that displays before the images are
loaded, e.g. Gmail often loads images after a user action, so take the time to
look at your design with placeholder text instead of images

- many clients, including Gmail, won't support custom fonts (unless the user
has them already installed on their computer) - use web fonts to make the
design more appealing on some clients, but always have a fallback option like
`Arial` or `Helvetica` handy; check how your design looks with the fallback font

- if you'd like something slightly more fancy, like a video or an interactive
element, keep in mind it won't be supported in most email clients and simply
link to it in your email in an appealing way

### Rethink flexibility
In web development, we're getting more and more used to flexible, seamless
designs that flow with the ever changing screen size of our devices. Emails,
even though they use the same basic blocks of HTML and CSS, are not that
advanced - to adjust for different devices, you need to make your design fluid
rather than flexible.

1. Some email clients don't support media queries - if you want to go from
two-column design to a single mobile column or even just adjust the font size,
keep in mind Android or any number of less popular mobile clients might not
agree with you.<br><br>
There are two main breakpoints in email - 600px for desktop and 320px for
mobile - but the most bulletproof way is to avoid them altogether, by either
using a device-agnostic single-column design or making your design fluid rather
than flexible. Think percentages, not breakpoints, define margins and paddings
rather than set sizes.

2. Don't expect emails to be pixel-perfect - you might really like those
rounded corners on your buttons, but making them work in older clients is going
to take a lot of work using images and careful placing, that might break on
some clients regardless of all the effort.<br><br>
Start with a simple, flat design and add improvements for more modern clients,
but don't expect them to work across all devices. Just make sure that adding
additional elements or flavors is not breaking your base design.

3. You can't control your users - some users adjust their setups and increase
the default fonts, make their displays monochrome or block images by default.
You won't know how your users see the email exactly and that's ok - if you
design with a fluid approach in mind, they'll still be able to read your
message and find the relevant information.


### Mind the quirks
There are many effects and elements you might be used to that cause
mind-boggling issues in HTML emails. Some of the more common ones are:

- custom fonts - you can get away with web fonts, but not all clients support
them; also, you have no control over user-specific settings, e.g. if they're
using an increased font in Android, the email client might render it regardless
of all your efforts to avoid it

- overlapping elements/layers - you can't overlap elements in emails, other
than adding a background image, and even that might not work in some clients,
but careful use of whitespace might help you achieve a similar effect

- putting text in symmetrical shapes - there are no flexible CSS properties to
ensure the shape will stay symmetrical, so it's better to use ovals than
circles and rectangles than squares

- motion/animation - email clients don't support JavaScript and most of them
don't work well with CSS animations, so there is virtually no way to make them
interactive

- hiding overflow - if you want to truncate text, do it before sending the
email, as hiding overflow might be problematic

- gradients - they won't display in many clients and can cause a solid
background color to obscure text

- non-image shapes other than rectangles - some email clients won't support
border-radius for circles or rounded corners, most don't play well with more
complicated shapes - try to keep it simple with rectangles and/or images

This is not an exhaustive list, but it can give you some ideas on where to
simplify your design.

### Reading code
If you have some experience with regular web development, you might expect the
code to be written in a certain way. HTML email code breaks many of the best
practices one gets used to.

1. In modern web development, using the `!important`  keyword too much (or
ever) or inlining styles are often considered code smells. In HTML emails they
might the only way to display the email as intended - it's not uncommon to see
an inlined style with `!important`,  just to force this one stubborn email
client to render it properly.

2. Email code is not DRY - you can define both a `background` property on a
table and add a declaration for the `background` and `background-image`  in the
inlined style for the same element, just because some clients support the table
property, some understand only the `background` shorthand and there are those
quirky ones, that only render the `background-image` if forced by a more
specific declaration. Oh, and don't forget about this class that also sets the
`background` with `!important`.

3. Arranging elements in email uses tables, which is the most widely supported
HTML element across all clients. If you want to make sure your layout is going
to work, the number of tables within tables can increase dramatically with each
element.

## Summary
Designing emails is different from designing websites - email clients have at
best patchy support for CSS and often require writing code that breaks the best
practices of web development.

1. Understand your users - have a list of email clients you want to support,
understand what to include in your emails and how to appeal to your user base.
Decide if you want to design mobile or desktop first.

2. Focus on the content - write a plain text email first, structuring it by
using paragraphs, bullet points, links. Make sure to always include plain text
version of your message.

3. Add the design gradually - the simpler the email, the better the chance it's
going to work well on multiple clients. Add in fallback for your features
(images, fonts, videos) and make sure to check how your message looks using
only the fallbacks.

4. Rethink flexibility - think fluid, not flexible. You can't control how your
user is going to see the design, so let go of pixel-perfect and make sure to
design with percentages and paddings, rather than set sizes.

5. Mind the quirks - the simpler, the better. Email clients have many quirks
and the more complicated the design, the higher the chance it's going to look
bad in some clients.

6. Reading code - email clients require you to write code that's not in line
with web development best practices. Get used to `!important` and loads of
tables.

### Useful links
1. Xfive [10 Tips for Better Email
Design](https://www.xfive.co/blog/10-tips-better-email-design/)
2. Tuts+ [The Complete Guide to Designing for
Email](https://webdesign.tutsplus.com/articles/the-complete-guide-to-designing-for-email--webdesign-12941)
3. Sigstr [How People ACTUALLY Read Your
Emails](https://www.sigstr.com/2017/05/23/email-structure-scientifically-backed-guide/)
4. Campaign Monitor [CSS Support Guide for Email
Clients](https://www.campaignmonitor.com/css/)
5. MailChimp [Email Design Guide](https://mailchimp.com/email-design-guide/)
6. Webdesigner Depot [The ultimate guide to email
design](https://www.webdesignerdepot.com/2014/06/the-ultimate-guide-to-email-design/)

<hr>
<p style="font-size: 14px">*though it is sometimes necessary to support IE8, but that's mostly because target user group uses outdated software</p>
