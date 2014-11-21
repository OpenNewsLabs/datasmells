# The Bed of Procrustes

In Greek mythology, [Procrustes](https://en.wikipedia.org/wiki/Procrustes) was a demented blacksmith who would torture his victims by "fitting" them to an iron bed by either stretching them if they were too short or chopping off their legs if they were too long. He was finally killed on his own rack by the hero Theseus, but his legacy lives on in some modern data exporters.

## Description of Error

This error applies to any scenario where raw data has either been truncated or expanded because of programming assumptions and limitations along the way. This error can apply to both individual records and collections of records. If data has been expanded to fit a larger conceptual space, this error can be corrected, but it can't be fixed if data is missed due to truncation.

## Some Examples

The most common type of Procrustean errors manifest themselves as limits on the number of rows in a database of records. Any dataset that is exactly a multiple of 100, 25 or other such divisors is suspect, but there are cases where counting the number of rows will not be enough. For instance, imagine a database with `N` categories and a bad exporter that dumps only the top 20 rows for each category. If there are more than 20 records assigned to each category, the database would be a suspicious `20*N` records in size. But, if some of the categories had less than 20 records associated with them, the total number of records in the database would not be necessarily suspicious. The only way this error would be revealed would be to look at the distibution of records per category and notice that it's artificially capped at 20 instead of allowing a more natural spread.

In many other cases, the data is truncated because of global limits and not the result of any bad exporters. For instance,

* Older versions of Excel will limit files to 65,536 rows or less. This is a famous sign that something is horribly wrong with your data.
* The DBF file format allows a maximum of 2GB files
* Older versions of WinZip would skip archiving files larger than 4GB

Finally, sometimes data within individual records may be resized in inappropriate ways. This is usually a result of Faulty Type Coercion[citation needed], where a data field is interpreted in the context of a different type as part of intermediary loading steps. For instance, I've seen these types of errors in databases:

* Text fields are not sufficiently large and some data is truncated on storage
* Years being interpreted as floats (2014.0)
* FIPS codes being turned into integers and losing the leading 0

# How To Fix

This is generally a problem that can't be corrected. If data is truncated as part of the process of exporting or processing the original dataset, there is no way to get that data back. Furthermore, the data that is available in the database may not necessarily be a representative sample of the entire database. For instance, if our contrived category exporter were dumping the 20 records for each category sorted by another column like size, it would be impossible to get the average value for any category. If you encounter this problem in your data, your only recourse will be to go to the original clean dataset if possible or figure out how badly this issue demolishes some of the assumptions about your data.