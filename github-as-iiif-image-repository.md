# Github as an IIIF Image Repository

This document describes an experimental approach for hosting IIIF images in a Github repository.  In this approach image and IIIF properties files are stored in a Github repository and served by an external service that dynamically generates IIIF presentation manifests and serves IIIF image tiles from the Github content.

Where possible, users are encouraged to use a purpose-built image sharing site for hosting and sharing image files.  [Wikimedia Commons](https://commons.wikimedia.org/wiki/Main_Page) is one such (free-to-use) site for hosting and sharing media files.  While Wikimedia Commons (and some other file sharing sites) do not yet support IIIF, the same service that is used to generate IIIF manifests and serve images from Github repositories also works with many of these sites.  The advantage of using images from a site like Wikimedia Commons is that IIIF manifests can be automatically generated using metadata obtained from Application Programming Interfaces (APIs) provided by the sites.  As an example, an IIIF manifest for the [https://commons.wikimedia.org/wiki/File:White-cheeked_Honeyeater_-_Maddens_Plains.jpg](https://commons.wikimedia.org/wiki/File:White-cheeked_Honeyeater_-_Maddens_Plains.jpg) image hosted by Wikimedia Commons is automatically generated using the URL [https://iiif.visual-essays.net/wc:White-cheeked_Honeyeater_-_Maddens_Plains.jpg/manifest.json](https://iiif.visual-essays.net/wc:White-cheeked_Honeyeater_-_Maddens_Plains.jpg/manifest.json).  Using the auo-generated IIIF manifest the image can easily be viewed in an IIIF compatible viewer or used by any IIIF-enabled tool.  The White-cheeked Honeyeater image can be seen in the iiif.visual-essays.net image viewer at [https://iiif.visual-essays.net/wc:White-cheeked_Honeyeater_-_Maddens_Plains.jpg](https://iiif.visual-essays.net/wc:White-cheeked_Honeyeater_-_Maddens_Plains.jpg)

In many cases a minimal set of IIIF required metadata for a single image can be inferred from site-wide default values and image-specific metadata that can be inferred from the image file name (such as the image title and reuse rights).

In situations where more descriptive metadata is needed a simple key-value properties file can be associated with a single image or a group of images located in a folder.

## The International Image Interoperability Framework (IIIF)

[IIIF](https://iiif.io) is a set of open standards for delivering high-quality, attributed digital objects online at scale. It’s also an international community developing and implementing the IIIF APIs. IIIF is backed by a consortium of leading cultural institutions.

The metadata in this repository are used to generate manifest files that are compliant with the [IIIF Presentation API 3.0](https://iiif.io/api/presentation/3.0/).  Detailed knowledge of IIIF is not needed to contribute or use images in this repository.

# Options for image and/or metadata hosting

## Image only

The easiest method for hosting a file is to simply upload the image file in this repository using a file name that can be used to generate the image label (caption) and can optionally includes a reuse rights code.

When using this method the image file name represents the image label and an optional segment that consists of a reuse rights code.  The label and rights code segments are separated by the first `-` character in the file name.  All spaces in the label segment of the file name must be converted to underscore (`_`) characters.  A double underscore (`__`) can be used to signify the end of a title.  This can be useful if multiple images in a folder will use the same label.  For instance, `Some_label__1-CC0.jpg` and `Some_label__2-CC0.jpg`.  In this example both image files have a unique file name but can be associated with the same label and rights code.

The reuse rights for an image is defined by appending a Creative Commons or Rights Statements code to the end of the file name (but before the file extension).  The reuse rights code is one of the [Creative Commons](https://creativecommons.org/licenses/) or [RightsStatements.org](https://rightsstatements.org/page/1.0/) codes defined in [Reuse Rights](#reuse-rights) section below.  If a rights code is not provided in the file name a default value is used.  The default value is obtained from properties files found in the folder hierarchy in which the image file is located.  If rights metadata is available in multiple properties files the value in the file that is closest to the image takes precedence.  For instance, if a properties file at the root of the repository defined the reuse rights as `CC BY` and then another properties file in a parent/ancestor folder of the image defined the reuse rights as `CC BY-SA`, the `CC BY SA` value would take precedence and would be used in the generated IIIF manifest.

### Examples

- The file name for the image [Fallstaff_Hotel_and_Westgate_Towers,_Canterbury-CC0.jpg](Fallstaff_Hotel_and_Westgate_Towers,_Canterbury-CC0.jpg) is formatted to enable a label and rights statement to be extracted without the need for a separate properties file.
  - The IIIF manifest generated for this image can be seen at [https://iiif.visual-essays.net/gh:kent-map/images/Fallstaff_Hotel_and_Westgate_Towers,_Canterbury-CC0.jpg/manifest.json](https://iiif.visual-essays.net/gh:kent-map/images/Fallstaff_Hotel_and_Westgate_Towers,_Canterbury-CC0.jpg/manifest.json).
  - This manifest can then be used in any IIIF viewer to view the image.  For instance, using the iiif.visual-essays.net viewer [https://iiif.visual-essays.net/gh:kent-map/images/Fallstaff_Hotel_and_Westgate_Towers,_Canterbury-CC0.jpg](https://iiif.visual-essays.net/gh:kent-map/images/Fallstaff_Hotel_and_Westgate_Towers,_Canterbury-CC0.jpg).

## Image and properties file

In situations where it is desired to associate richer metadata with an image a supplemental properties file may be used.

### Examples

- The image [Dane_John_Park.jpg](Dane_John_Park.jpg) is accompanied with the [Dane_John_Park.yaml](Dane_John_Park.yaml) properties file which includes a summary, date, and Wikidata QID (in the `depicts` metadata field).
  - The IIIF manifest for this image can be seen at [https://iiif.visual-essays.net/gh:kent-map/images/Dane_John_Park.jpg/manifest.json](https://iiif.visual-essays.net/gh:kent-map/images/Dane_John_Park/manifest.json).
  - The image can be viewed in the iiif.visual-essays.net image viewer at [https://iiif.visual-essays.net/gh:kent-map/images/Dane_John_Park.jpg](https://iiif.visual-essays.net/gh:kent-map/images/Dane_John_Park.jpg).

> Where possible, it is recommended to include the `depicts` metadata property to identify entities depicted in the image.  In this example the included Wikidata QID ([Q16988443](https://www.wikidata.org/wiki/Q16988443)) indicates that the [Dane John Mound](https://en.wikipedia.org/wiki/Dane_John_Mound) is depicted in the image.

## Properties file only

In situations where an image is hosted on another web site that does not provide a IIIF manifest a properties file is used to define the IIIF metadata in a generated IIIF manifest.  When using this method the URL to the externally hosted image is included in the properties file in addition to the usual IIIF metadata properties.

### Examples

- [Canterbury_Cathedral_2021.yaml](Canterbury_Cathedral_2021.yaml)
  - Generated manifest: [https://iiif.visual-essays.net/gh:kent-map/images/Canterbury_Cathedral_2021/manifest.json](https://iiif.visual-essays.net/gh:kent-map/images/Canterbury_Cathedral_2021/manifest.json)
  - Image viewer URL: [https://iiif.visual-essays.net/gh:kent-map/images/Canterbury_Cathedral_2021](https://iiif.visual-essays.net/gh:kent-map/images/Canterbury_Cathedral_2021)
- [Canterbury_Cathedral_circa_1905.yaml](Canterbury_Cathedral_circa_1905.yaml)
  - Generated manifest: [https://iiif.visual-essays.net/gh:kent-map/images/Canterbury_Cathedral_circa_1905/manifest.json](https://iiif.visual-essays.net/gh:kent-map/images/Canterbury_Cathedral_circa_1905/manifest.json)
  - Image viewer URL: [https://iiif.visual-essays.net/gh:kent-map/images/Canterbury_Cathedral_circa_1905](https://iiif.visual-essays.net/gh:kent-map/images/Canterbury_Cathedral_circa_1905)

# Reuse Rights

The IIIF Presentation Manifests provide a flexible approach for explicitly defining the reuse rights for an image and any attribution (or other) statements that must be displayed when the image is used.

`Rights` refers string that identifies a license or rights statement that applies to the content of the resource, such as the JSON of a Manifest or the pixels of an image. The value must be drawn from the set of [Creative Commons](https://creativecommons.org/licenses/) license URIs or [RightsStatements.org](https://rightsstatements.org/page/1.0/) rights statement URIs

## Creative Commons Licenses

- `CC0`: [Public Domain Dedication](http://creativecommons.org/publicdomain/zero/1.0/)
- `CC-BY`: [Attribution](http://creativecommons.org/licenses/by/4.0/)
- `CC-BY-SA`: [Attribution-ShareAlike](http://creativecommons.org/licenses/by-sa/4.0/)
- `CC-BY-ND`: [Attribution-NoDerivs](http://creativecommons.org/licenses/by-nd/4.0/)
- `CC-BY-NC`: [Attribution-NonCommercial](http://creativecommons.org/licenses/by-nc/4.0/)
- `CC-BY-NC-SA`: [Attribution-NonCommercial](http://creativecommons.org/licenses/by-nc-sa/4.0/)
- `CC-BY-NC-ND`: [Attribution-NonCommercial-NoDerivs](http://creativecommons.org/licenses/by-nc-nd/4.0/)

## Rights Statements

- `InC`: [In Copyright](http://rightsstatements.org/vocab/InC/1.0/)
- `InC-OW-EU`: [In Copyright - EU Orphan Work](http://rightsstatements.org/vocab/InC-OW-EU/1.0/)
- `InC-EDU`: [In Copyright - Educational Use Permitted](http://rightsstatements.org/vocab/InC-EDU/1.0/)
- `InC-NC`: [In Copyright - Non-Commercial Use Permitted](http://rightsstatements.org/vocab/InC-NC/1.0/)
- `InC-RUU`: [In Copyright - Rights-Holder(s) Unlocatable or Unidentifiable](http://rightsstatements.org/vocab/InC-RUU/1.0/)
- `NoC-CR`: [No Copyright - Contractual Restrictions](http://rightsstatements.org/vocab/NoC-CR/1.0/)
- `NoC-NC`: [No Copyright - Non-Commercial Use Only](http://rightsstatements.org/vocab/NoC-NC/1.0/)
- `NoC-OKLR`: [No Copyright - Other Known Legal Restrictions](http://rightsstatements.org/vocab/NoC-OKLR/1.0/)
- `NoC-US`: [No Copyright - United States](http://rightsstatements.org/vocab/NoC-US/1.0/)
- `CNE`: [Copyright Not Evaluated](http://rightsstatements.org/vocab/CNE/1.0/)
- `UND`: [Copyright Undertermined](http://rightsstatements.org/vocab/UND/1.0/)
- `NKC`: [No Known Copyright](http://rightsstatements.org/vocab/NKC/1.0/)
