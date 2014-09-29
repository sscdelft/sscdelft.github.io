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

News items live in the `_posts` directory.  The file containing the news should
be named `YYYY-MM-DD-the-news-header.md` where `YYYY-DD-MM` is today's date and
`the-news-header` should be the, possibly shortened, title of the news item in
using only alphanumeric characters and dashes.  The yaml should contain `layout:
post`, `category: news` and a `title` attribute.  An example news item:

    ---
    layout: post
    category: news
    title: example news item
    ---

    news text

News items are automatically listed in `news.html` and the atom feed `atom.xml`.

Guidelines:

*   News items should not be rewritten or renamed!  Fixing typos is allowed
    though.

*   A news item related to an activity, see below, should link to the activity
    page.  An example:

        ---
        layout: post
        category: news
        title: Announcing An Event
        ---

        On DD MM the Student Chapter will organise [An Event]!

        [An Event]: {{ site.baseurl }}/activities/YYYY-MM-DD/an-event.html

Activities
----------

Activities are exactly the same as news items, except for the category, which
should be `activities`.  An example:

    ---
    layout: post
    category: activities
    title: example activity
    ---

    activity text

Add the yaml attribute `discontinued: true` when the activity has been
discontinued:

    ---
    layout: post
    category: activities
    title: example activity
    discontinued: true
    ---

    activity text

Activities are listed in `activities.html`, active activities on top,
discontinued activities at the bottom, collapsed.

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
    category: news
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
