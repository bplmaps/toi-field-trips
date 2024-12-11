In the final practicum project, you will be drawing on the technical knowledge developed in the Mini-Projects, including working in a GitHub code repository, formatting documents using Markdown, and drawing on digital resources using the IIIF protocol. While the emphasis of this project is primarily on your interpretive voice, research skills, and critical approach towards “place-based” study, 5 points of your final grade will be based on your ability to work with these digital publishing tools—as well as your ability to experiment and problem-solve in a technical capacity.

## Accessing the repository

The code repository for the site we will be building together is found on GitHub at <http://github.com/garrettdashnelson/hist-7250-amrev>. You’ll likely find it has a familiar structure, since it is a clone of the _Walk to the Sea_ website, with modifications specifically for this project. Like the `panel-truck-shell` repository from Mini-Project 2, this repository is part of my personal account, so you will be able to edit files directly, without making a fork. (After the project is complete, I will migrate the code into the `bplmaps` GitHub account.)
 
All of the files you will be working with are found in the `/content` directory of the repository. Each of you will create a single Markdown file in the `/content/locations` folder with frontmatter and content. This will serve as the key page which creates your “stop” in the published site.

Find the file `/content/locations/copley-square.md`. This is a dummy example which I’ve created. You’ll notice that the frontmatter of the file looks like this:

``` 
---
title: TEST Copley Square
metaDescription: This is a test.
description: Copley Square is not actually very important for the American Revolution. This is just a test page.
order: 0
latitude: 42.349742871002285
longitude: -71.07651991800763
resources:
- demo-resource
---
```

To get started, create a new file in the `/content/locations` directory with the filename `your-location.md`, replacing `your-location` with a string that describes your stop. This file name (called the “slug”) will eventually become the URL of the page for your stop, without the `.md` file suffix.

Copy and paste the frontmatter from `copley-square.md` to get started, and then replace all of the fields with the appropriate information for your stop. Leave the `order` field set to `0` for now. And, at first, you can leave the `resources` array populated with a single dummy entry, `test-resource`— you’ll come back to this later.

You’ll need to replace the `title`, `metaDescription`, and `description` fields. `title` and `description` will appear in the published version of your stop, while `metaDescription` is used for search engine summaries of the page.

You will also need to fill in the `latitude` and `longitude` fields for where you would like the single pinned location of your stop to appear on the map. There are many different online tools for finding latitude and longitude points, but the simplest is to go to Google Maps, right-click on the location where you want to place a pin, and then copy the coordinate pair from the drop-down menu where you have right clicked.

After you’ve populated the frontmatter, create a few Markdown headers in the body content area of the Markdown file, and then commit it to the repository. After the site rebuilds (within 5 minutes), you should see the new page created at <https://hist-7250-amrev.netlify.app/>. 

### Adding zoomable images

In the `copley-square.md` dummy stop, you’ll see two zoomable images. These correspond in the body content of the Markdown file with two custom tags that use the `<zoomable-image>` syntax. This custom syntax allows you to embed images that pop up as lightbox features and, if they are powered by IIIF services, allow the user to deep zoom.

The `<zoomable-image>` tag has several required parameters. The `type`  parameter is used to choose between a IIIF image (typically from an institutional digital collection, like Digital Commonwealth) or a “static” image (in other words, an image file ending with a file suffix like `.jpg` or `.png` from anywhere on the web). 

To use a IIIF image, set the `type` parameter to `iiif`. Up until this point, you have been working with IIIF _Manifests_. A Manifest includes _all_ the information associated with a single digital object, including its metadata and as many images as the object has associated with it. (You might remember when using the panel-truck screenplay studio that you would always leave the sequence, canvas, and image variables set to 0, because in most map digital collection there is just a single image associated with the object.)

In this case, we do not want the Manifest, but rather the IIIF _Image_ service. A Manifest can include any number of Image resources, though with a single-sheet map there is usually just a single Image. To find the IIIF Image URLs associated with a IIIF Manifest, use the Leventhal Center’s IIIF Tools at \<[http://geoservices.leventhalmap.org/iiif-tools/](http://geoservices.leventhalmap.org/iiif-tools/)\>. Copy and past the Manifest for an object you’d like to work with—for example, 
 
### Adding a panel-truck interactive

You’ll also notice the dummy example has a panel-truck interactive embedded within in it. If you would like to add a panel-truck into your stop, you can create the new screenplay using exactly the same method as in Mini-Project 2, by adding a new JSON file to the GitHub repository at [https://github.com/garrettdashnelson/panel-truck-shell/](https://github.com/garrettdashnelson/panel-truck-shell/). Then, you can embed this viewer into your stop with the following code:

```
<iframe src="https://garrettdashnelson.github.io/panel-truck-shell/#{{your-screenplay}}" width="100%" height="500">
</iframe>
```

Replace the field `{{your-screenplay}}` with the filename of `your-screnplay.json` in the `panel-truck-shell` repository.


### Adding “other resources” 

You’ll remember that we left an example `test-resource` in the frontmatter `resources` array. This array populates a set of “additional resources” that appear below the end of the main body content. The reason that these are schematized as linked resources rather than lists embedded directly in the body content is because of the possibility that two different stops might want to share the same resource. (This behavior is inherited from _Walk to the Sea_.) 

To create a new resource, add a file to the `/content/resources` directory of the repository called `{{your-resource}}.md`, replacing the field with a string name for your resource (make sure not to use spaces or special characters). Resources do not have any body content, but simply three front matter fields that look like this:

```
---
title: "TEST Boston Public Library"
description: "When you're in Copley Square, you may want to visit the BPL"
link: "https://www.bpl.org"
---
```

You should replace these three fields with the information for your resource. Then, back in the frontmatter of your stop page, add `your-resource` to the `resources ` array.

⚠️  If you put a quotation mark in a simple frontmatter string, it will break the syntax, because it will look like you’re trying to close the string. For instance, this will break:

```
---
title: "A broken resource"
description: "This resource "won't work" because there's a quotation mark"
link: "https://www.bpl.org"
---
```

You will need to _escape_ the quotation mark like this:

```
---
title: "A valid resource"
description: "This resource \"will work\" because I escaped the quotation marks"
link: "https://www.bpl.org"
---
```

Alternatively, you can use a special block string mark in your front matter, like this:

```
---
title: "A valid resource"
description: >
    This resource "will work" because the string is formatted using a block syntax.
link: "https://www.bpl.org"
---
```


### The rest of the Markdown toolkit

I encourage you to use the custom `<zoomable-image>` tag for adding images, but you _can_ still use the standard Markdown syntax for images that you learned in Mini-Project 1, e.g. `![Caption](https://source.to/image.jpg)`. You can and should use headers and other Markdown formatting to structure the remainder of your content.
