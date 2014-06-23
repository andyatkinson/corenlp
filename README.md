# Corenlp

Corenlp is a Ruby gem that uses the [Stanford CoreNLP Java tools](http://nlp.stanford.edu/software/corenlp.shtml) to parse text. The gem takes the output from Stanford CoreNLP and builds objects in Ruby, for use in Ruby applications.

Stanford CoreNLP requires Java version 1.6 or higher. Installations vary so that will need to be installed by the developer on their own before continuing.

Development has been done with 64-bit Java on a OS X machine. 3G of RAM is allocated for the Java process, which can be set any time the parser is called.

The following Java version has been used in development.

    $ java -version
    java version "1.7.0_45"
    Java(TM) SE Runtime Environment (build 1.7.0_45-b18)
    Java HotSpot(TM) 64-Bit Server VM (build 24.45-b08, mixed mode)

## Installing Stanford CoreNLP

Run `rake corenlp:download_deps` to download the Stanford CoreNLP dependencies. The files will be extracted to the `lib/ext` directory, which is ignored from git. The dependencies directory can be customized. The download URL can also be customized.

The rake task will use the values from the following environment variables if they are set.

 * `CORENLP_DOWNLOAD_URL` - This is set to "http://nlp.stanford.edu/software/stanford-corenlp-full-2014-06-16.zip" which was the latest version when this was written.
 * `CORENLP_DEPS_DIR` - This is set to "./lib/ext/", which is a directory that exists in our project where we want to place the Stanford CoreNLP files.

To customize these values, supply environment variable arguments when calling the rake task like this:

    rake corenlp:download_deps CORENLP_DEPS_DIR='./my_directory'

## Testing the output in a IRB console

Corenlp gem builds up a treebank of structured parts that define tokens, sentences, and dependencies between the tokens. This treebank structure is represented as a nested Ruby hash. Token objects that are part of a sentence are nested within the sentence. Token dependencies are nested within the sentence, and so on.

The following code will build up a treebank structure for the raw text "Put the book down.". On my machine this takes around 10 seconds to run.

    bundle exec irb
    Bundler.require
    Corenlp::Treebank.new(raw_text: "Put the book down.").parse

## Options

The Treebank object can be initialize with various options.

 * `java_max_memory` - set to 3GB by default. This can be customized via the Treebank initializer to be `-Xmx2g`, which would use a max of 2GB of memory, for example.
 * `threads_to_use` - number of threads Stanford CoreNLP uses to parse text. This is set to 4 by default. This option is passed to the Java executable.
 * `output_directory` - by default this is `./tmp/language_processing`, which already exists. This is where Stanford CoreNLP XML files are placed. These XML files represented the structured parser output.

## Tests

Minitest is used as a test suite for the Ruby objects. New code should include test coverage. Manually testing is also useful. Internally we have some more test methods to verify parser output on the same content over time, but they are not included at this time.

    rake

To run a single test:

    ruby path/to/file.rb --name test_method_name

## Terminology

Stanford CoreNLP uses a lot of terminology from the natural language processing field, and defines its own terminology. Refer to the [Stanford CoreNLP documentation](http://nlp.stanford.edu/software/corenlp.shtml) to learn more.

## Contributors

This gem was developed at Lengio by Andy Atkinson as an extraction of some of our natural language processing tools.

  * Andy Atkinson, gem author and maintainer
  * Kamran Khan
  * Rodolfo Carvalho

## Rubygems badge

[![Gem Version](https://badge.fury.io/rb/corenlp.svg)](http://badge.fury.io/rb/corenlp)

## License

The MIT License (MIT)

Copyright (c) 2014 Lengio Corporation

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
