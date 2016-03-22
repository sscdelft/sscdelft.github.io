Our webpage sscdelft.github.io
===============================

This is the webpage of the SIAM Student Chapter Delft.
It uses the theme by [Matt Graham] and it's hosted on GitHub Pages.

For more information please visit http://sscdelft.github.io
or write us an email: SIAMSC-EWI@tudelft.nl

[Matt Graham]: https://twitter.com/michigangraham

Maintainers' guide
==================

This site uses the *jekyll* templating engine.  To build the site locally run

    jekyll build

or

    jekyll serve --watch

The latter spawns a webserver (`serve`) and rebuilds the site automatically when
the source files have been modified (`--watch`).  If you have installed jekyll
via *bundler*, prefix the commands with `bundle exec`.

Navigation
----------

Pages containing a `navigation` attribute in the yaml are listed in the
navigation bar.  The order is based on the value of the `navigation` attribute.

News
----

News items live in the `news/_posts/` directory.  The file containing the news
should be named `YYYY-MM-DD-the-news-header.md` where `YYYY-DD-MM` is today's
date and `the-news-header` should be the, possibly shortened, title of the news
item in using only alphanumeric characters and dashes.  The yaml should contain
`layout: post` and a `title` attribute.  An example news item:

    ---
    layout: post
    title: example news item
    ---

    news text

News items are automatically listed in `news/` and the atom feed `atom.xml`.

Guidelines:

*   News items should not be rewritten or renamed!  Fixing typos is allowed
    though.

*   A news item related to an activity, see below, should link to the activity
    page.  An example:

        ---
        layout: post
        title: Announcing An Event
        ---

        On DD MM the Student Chapter will organise [An Event]!

        [An Event]: {{ site.baseurl }}/activities/YYYY-MM-DD/an-event.html


Semi-automatically mailing news items
-------------------------------------

The python3 script `mail-news` can automatically send news items to all the
members of the student chapter (based on the [members repository]).  The html
layout of the mail is located in `_layouts/mail-news.html`.  The script requires
that there are no local changes in the working tree or repository with respect
to the [main repository] to make sure that the news items have been published
and are not going to be changed.

Run the script without arguments in the root of the repository,

    ./mail-news

The script loops over all unset news items — a news item is sent if there is a
file in the folder `_sent_mail/` with the same name as the news item — asks
your permission to send a mail and stores a file in the `_sent_mail` directory.
Finally, the script commits the `_sent_mail/` folder and asks you to manually
push this commit to the [main repository].

To test mailing news items run the script with extra arguments `--test` and your
email address, e.g.

    ./mail-news --test your.email.address@some.domain

In the 'test' mode local changes in the working tree or repository are allowed,
mail will be sent to the supplied address only, not to the members, and news
items will not be marked as sent, i.e. the `_sent_mail/` directory will not be
updated.

[members repository]: https://github.com/sscdelft/Members
[main repository]: https://github.com/sscdelft/sscdelft.github.io

Activities
----------

Activities are exactly the same as news items, except they are located in
`activities/_posts/`.  An example:

    ---
    layout: post
    title: example activity
    ---

    activity text

Add the yaml attribute `discontinued: true` when the activity has been
discontinued:

    ---
    layout: post
    title: example activity
    discontinued: true
    ---

    activity text

Activities are listed in `activities/`, active activities on top, discontinued
activities at the bottom, collapsed.

Guidelines:

*   Choose a single, preferably short name for an activity and use this name
    consistently on the activity page and related news items.  The activity page
    title should be given the name of the activity.  Do not add a specific date
    or location in the title of the activity page.

*   Activities should not be removed.  If activities are discontinued add
    `discontinued: true` to the yaml.

Photos
------

Guidelines:

*   Reduce the quality of photos, either by downscaling or by lowering the jpeg
    encoding quality.  Several hundred kilobytes per photo should be enough.

    To resize a set of photos to full HD resolution using *imagemagick*, use
    the following command:

        for f in *.jpg; do
            echo "$f"
            convert "$f" -resize '1920x1080>' -quality 80 -interlace Plane "$f"
        done

    Note: The photos will be overwritten!

*   Place all photos of an event in a single directory, along with the
    thumbnails, see below.

Thumbnails
----------

You can use *imagemagick* to create thumbnails.  For a single image use the
following command:

    f=photo.jpg convert "$f" -thumbnail 128x128^ -gravity center\
        -extent 128x128 -strip -interlace Plane -quality 80\
        `echo "$f" | sed 's/.[^.]\+$/-thumb.jpg/'`

Processing multiple files can be done as follows:

    for f in *.jpg; do
        (echo "$f" | grep -vq '.*-thumb\.jpg$') &&
        (
            echo "$f"
            convert "$f" -thumbnail 128x128^ -gravity center\
                -extent 128x128 -strip -interlace Plane -quality 80\
                `echo "$f" | sed 's/.[^.]\+$/-thumb.jpg/'`
        )
    done

Attaching media to posts (news, activities)
-------------------------------------------

Add an `attached_media` attribute to the yaml of the post with a list of images.
Each item should be a disctionary with the following keys: `url`, `type` and
`thumb`.  Currently the only supported `type` is `image`.  The `thumb` should
point to a square thumbnail, preferably of 128 pixels, using a low quality jpeg
encoding.  An example:

    ---
    layout: post
    title: example post
    attached_media:
      - url: "{{ site.baseurl }}/images/YYYY-MM-DD-event/0000.jpg"
        type: image
        thumb: "{{ site.baseurl }}/images/YYYY-MM-DD-event/0000-thumb.jpg"
      - url: "{{ site.baseurl }}/images/YYYY-MM-DD-event/0001.jpg"
        type: image
        thumb: "{{ site.baseurl }}/images/YYYY-MM-DD-event/0001-thumb.jpg"
      - url: "{{ site.baseurl }}/images/YYYY-MM-DD-event/0002.jpg"
        type: image
        thumb: "{{ site.baseurl }}/images/YYYY-MM-DD-event/0002-thumb.jpg"
    ---

    news text
