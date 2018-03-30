---
layout: post
---

# Diving into AIPs: ContentDM ➜ Archivematica

![Picture of Diving Operation](/weaverblog/resources/diving.png)

*Underwater diving operations, ca. 1900. [Source](http://content.libraries.wsu.edu/cdm/singleitem/collection/cpratsch/id/359/rec/1)*
 
My institution has a LOT of amazing content preserved digitally in its storage. It also has a LOT of amazing metadata about this content, much of it contained within a hosted CONTENTdm instance, that surely represents thousands of hours of labor. One of my ongoing goals is to start gathering all of this information into archival packages built around the [OAIS model](https://en.wikipedia.org/wiki/Open_Archival_Information_System). This would make it not only easier to handle in terms of performing preservation actions, it also would help ensure that digital items never become disassociated from their respective descriptive and administrative metadata.

Recently, the main tool I have been experimenting with is [Archivematica](https://www.archivematica.org/en/). So far, Archivematica seems to have a lot of potential for assisting with what I am trying to do. It has a logical [microservice](https://en.wikipedia.org/wiki/Microservices) based approach that should open the door to a lot of customization. Additionally, like the fact that Archivematica is designed to leverage the power of the open-source community and is working to integrate other amazing projects such as [MediaConch](https://mediaarea.net/MediaConch).

All this being said, I was curious to see what a hypothetical process of wrangling our existing data into Archivematica would consist of. Conceptually, I need to export the metadata out of CONTENTdm, associate it with preservation files in our storage and then arrange the metadata/items in a manner that is palatable to Archivematica. I was also curious to see how well I could automate this process, as should we decide to do a migration of this sort for real I would rather not spend the next twelve months copy-pasting metadata and files by hand!

To evaluate the viability of this process, I created a test script (current form available [here on github](https://github.com/privatezero/contentdm_parse/blob/master/contentdm_parse.rb)). Using the [Ruby CSV class](https://ruby-doc.org/stdlib-2.0.0/libdoc/csv/rdoc/CSV.html) this script attempts to take the TSV (tab separated vales) metadata exports provided by CONTENTdm and parse it into the CSV (comma separated values) Archivematica allows for [Dublin Core ingest](https://wiki.archivematica.org/Metadata_import). Although the two formats use different column headers, since they map to the same Dublin Core fields, this was a relatively smooth process. Since the CONTENTdm metadata also included information about original item IDs , I was also able to parse this column and have the script recursively search directories in our digital storage to find archival master files that corresponded to the access files in CONTENTdm.

Once the script has located the master files and parsed the metadata it is then able to assemble a package suitable for Archivematica ingest. Archivematica calls for a basic structure involving a `objects` directory and a `metadata` directory. For every master file that is located by the script, it makes a copy in an `output/objects` directory and then compares checksums for the two files to make sure that no data was corrupted It also exports the newly created metadata csv (including the relative file paths to the objects that Archivematica requires for metadata ingest) into `output/metadata`.

In tests so far, this has worked surprisingly well! I tried pointing the script at the [Charles Pratch Collection](http://content.libraries.wsu.edu/cdm/landingpage/collection/cpratsch/) of early 1900s Grays Harbor photographs, and not only did it perform entirely as expected, the resulting CSV file required very little manual inspection prior to ingest into my trial instance of Archivematica. 

I was able to go from a whole lot of this:

{% highlight xml %}
Title   Manning Hill and Eagle bike on road to Hoquiam, 1893.
Creator     Pratsch, Charles R.
Date    1893
Subject     Bicycles & tricycles--Washington (State); Men--Washington (State); Roads--Washington (State); Trees--Washington (State)
Type    Image
Genre   Glass negatives
Identifier  pc018b02n001
Source  Is found in PC 18, Charles R. Pratsch Photographs http://libraries.wsu.edu/masc/finders/pratsch.htm at Washington State University Libraries' Manuscripts, Archives, and Special Collections (MASC) http://libraries.wsu.edu/masc
Publisher   Manuscripts, Archives, and Special Collections, Washington State University Libraries: http://www.libraries.wsu.edu/masc/masc.htm
Coverage    Hoquiam, Washington
Rights  http://rightsstatements.org/vocab/NKC/1.0/
Rights Notes    No known copyright. Item went into public domain 70 years after the 1937 death of the author.
Format  Original photographic prints were scanned as 300 dpi TIFF files on a Microtek 9600XL scanner. 72 dpi JPEG files were then added to the CONTENTdm database at the WSU Libraries.
Language    English 
{% endhighlight %}

to a whole lot of this:

{% highlight xml %}
      <mets:xmlData>
        <dcterms:dublincore xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:dcterms="http://purl.org/dc/terms/" xsi:schemaLocation="http://purl.org/dc/terms/ http://dublincore.org/schemas/xmls/qdc/2008/02/11/dcterms.xsd">
          <dc:title>Manning Hill and Eagle bike on road to Hoquiam, 1893.</dc:title>
          <dc:creator>Pratsch, Charles R.</dc:creator>
          <dc:description></dc:description>
          <dc:date>1893</dc:date>
          <dc:subject>Bicycles &amp;amp; tricycles--Washington (State); Men--Washington (State); Roads--Washington (State); Trees--Washington (State)</dc:subject>
          <dc:type>Image</dc:type>
          <dc:identifer>pc018b02n001</dc:identifer>
          <dc:source>Is found in PC 18, Charles R. Pratsch Photographs http://libraries.wsu.edu/masc/finders/pratsch.htm at Washington State University Libraries' Manuscripts, Archives, and Special Collections (MASC) http://libraries.wsu.edu/masc</dc:source>
          <dc:publisher>Manuscripts, Archives, and Special Collections, Washington State University Libraries: http://www.libraries.wsu.edu/masc/masc.htm</dc:publisher>
          <dc:rights>http://rightsstatements.org/vocab/NKC/1.0/</dc:rights>
          <dc:format>Original photographic prints were scanned as 300 dpi TIFF files on a Microtek 9600XL scanner. 72 dpi JPEG files were then added to the CONTENTdm database at the WSU Libraries.</dc:format>
          <dc:language>English</dc:language>
        </dcterms:dublincore>
      </mets:xmlData>
    </mets:mdWrap>
  </mets:dmdSec>
{% endhighlight %}

While performing digital preservation actions on a whole lot of amazing things like this:

![Man with bike in woods](/weaverblog/resources/bikedude.png)

*Manning Hill and Eagle bike on road to Hoquiam. [Source](http://content.libraries.wsu.edu/cdm/singleitem/collection/cpratsch/id/0/rec/1)*

While I am still evaluating the different options for moving our digital storage into a package based preservation system, I have been very heartened by the results of this process! While this system is only effective so far for digital items that have been cataloged, it shows that an Archivematica migration is definitely a viable possibility for us. I look forward to seeing how much more of the process could potentially be automated!
