---
layout: post
---

# Diving into AIPs: ContentDM -> Archivematica

![Picture of Diving Operation](/weaverblog/resources/diving.png)

*Underwater diving operations, ca. 1900. [Source](http://content.libraries.wsu.edu/cdm/singleitem/collection/cpratsch/id/359/rec/1)*
 
My institution has a LOT of amazing content preserved digitally in its storage. It also has a LOT of amazing metadata about this content, much of it contained within a hosted CONTENTdm instance, that surely represents thousands of hours of labor. One of my ongoing goals is to start gathering all of this information into archival packages built around the [OAIS model](https://en.wikipedia.org/wiki/Open_Archival_Information_System). This would make it not only easier to handle in terms of performing preservation actions, it also would help ensure that digital items never become disassociated from their respective descriptive and administrative metadata.

Recently, the main tool I have been experimenting with is [Archivematica](https://www.archivematica.org/en/). So far, Archivematica seems to have a lot of potential for assisting with what I am trying to do. It has a logical[microservice](https://en.wikipedia.org/wiki/Microservices) based approach that should open the door to a lot of customization. Additionally, like the fact that Archivematica is designed to leverage the power of the open-source community and is working to integrate other amazing projects such as [MediaConch](https://mediaarea.net/MediaConch).

All this being said, I was curious to see what a hypothetical process of wrangling our existing data into Archivematica would consist of. Conceptually, I need to export the metadata out of CONTENTdm, associate it with preservation files in our storage and then arrange both metadata and items in a manner that is palatable to Archivematica.

Archivematica [Metadata ingest](https://wiki.archivematica.org/Metadata_import)

CONTENTdm [Metadata export](https://www.oclc.org/support/services/contentdm/help/collection-admin-help/exporting-metadata.en.html)

[Current Script](https://github.com/privatezero/contentdm_parse/blob/master/contentdm_parse.rb)

