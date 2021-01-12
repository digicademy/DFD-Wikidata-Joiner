# DFD-Wikidata-Joiner

A [KNIME Analytics Platform](https://www.knime.com/knime-analytics-platform) workflow for conservatively matching [DFD (Digital Dictionary of Surnames in Germany)](http://www.namenforschung.net/en/dfd/dictionary/list/) entries with [Wikidata](https://www.wikidata.org/) family name items. It outputs a list of matches ready for the [QuickStatements](https://tools.wmflabs.org/quickstatements/) bulk editing tool, which then adds the DFD-ID using the corresponding property [P6597](http://www.wikidata.org/entity/P6597) to the Wikidata items.

The workflow fetches a full list of currently published DFD articles and a list of family name items from Wikidata’s SPARQL endpoint. Family name items are first filtered in the SPARQL query to:
* exclude items which already have “[DFD-ID](http://www.wikidata.org/entity/P6597)”
* only items with the statement ”[instance of](http://www.wikidata.org/entity/P31): [family name](http://www.wikidata.org/entity/Q101352)”
* only items with the statement ”[writing system](http://www.wikidata.org/entity/P282): [Latin script](http://www.wikidata.org/entity/Q8229)”
* exclude items with additional other values for “[writing system](http://www.wikidata.org/entity/P282)”
* only items with a value for “[native label](http://www.wikidata.org/entity/P1705)”

Further filtering is done in KNIME, because it would be too costly for the Wikidata SPARQL endpoint:
* exclude items whose value of “[native label](http://www.wikidata.org/entity/P1705)” occurs on more than one family name item
* exclude items with more than one value of “[native label](http://www.wikidata.org/entity/P1705)”

This results in a list of items where unequivocal 1:1 matches between DFD and Wikidata are possible and a new statement with “[DFD-ID](http://www.wikidata.org/entity/P6597)” is unlikely to be erroneous or problematic.

The name forms from DFD are then joined to the native labels from Wikidata, using exact (and case-sensitive) string matching.

The resulting list of matches is transformed to the CSV format required by QuickStatements. The final result can be copy-and-pasted to [a new batch on QuickStatements](https://tools.wmflabs.org/quickstatements/#/batch/new). QuickStatements will then perform a batch of edits on Wikidata to add the statements.

## Download, Installation and Requirements

All releases can be downloaded as a .knwf file from the [release page of this repository](https://github.com/digicademy/DFD-Wikidata-Joiner/releases).

The .knwf file can then be imported in [KNIME version 4.2.3 or higher](https://www.knime.com/downloads/download-knime).

The [KNIME Semantic Web](https://hub.knime.com/knime/extensions/org.knime.features.semanticweb) extension is required. When importing the workflow, installation of this extension is automatically prompted. 

## License

The software is published under the terms of the MIT license.

## Research Software Engineering and Development

Copyright 2019 <a href="https://orcid.org/0000-0001-8483-8123">Julian Jarosch</a>

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

