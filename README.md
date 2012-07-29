ElasticSearch by Example
========================

_ElasticSearch by Example_ is a simple framework to learn and play with
[ElasticSearch](http://elasticsearch.org).

It will help you get started in [ElasticSearch](http://elasticsearch.org)
and provide a sandbox for experimentation and comparison of features.

Each example contains a info file with the key information on the feature,
sample data, a mapping file and a search query. You can see the output directly
into the console.

It is important that the program is kept as simple as possible.
A [ElasticSearch](http://elasticsearch.org) beginner should easily understand each step.

I have deliberately used raw [JSON](http://www.json.org/) structures to facilitate communication with [ElasticSearch](http://elasticsearch.org) using the
[REST](http://en.wikipedia.org/wiki/Representational_state_transfer) API to make
it clear. There are no abstractions to hide details.


Requirements
------------

* Ruby 1.9.3+


Installation
------------

Clone the project repository from [GitHub](http://github.com/):

    git clone git://github.com/rmoszczynski/elasticsearch-by-example.git
    cd elasticsearch-by-example
    bundle install

If you have already installed [ElasticSearch](http://elasticsearch.org) local
or on a remote machine you're done.

To install [ElasticSearch](http://elasticsearch.org) on Mac with
[Homebrew](http://mxcl.github.com/homebrew/) type:

    brewdle install


Configuration
-------------

_ElasticSearch by Example_ uses its own [ElasticSearch](http://elasticsearch.org) configuration file stored in `config/elasticsearch.yml`.

To use a remote [ElasticSearch](http://elasticsearch.org) installation
please customize the `host` and `port` constants in the `bin/run` script.


Usage
-----

To use _ElasticSearch by Example_ open two console windows.

Console 1:

    bundle exec foreman start

Console 2:

    bin/run queries/text

As you can see the `bin/run` script takes one parameter, a path to a selected
example directory relative to the `examples` directory in the root of this project.

You can list all examples by using the `list` parameter:

    bin/run list


Links
-----

* [Project Home](https://github.com/rmoszczynski/elasticsearch-by-example)
* [ElasticSearch Home](http://www.elasticsearch.org/)


Feedback
--------

You can send feedback via [GitHub Issues](https://github.com/rmoszczynski/elasticsearch-by-example/issues).


License
-------

    ElasticSearch by Example.

    All files in this project are licensed under the following license:

    Copyright (c) 2012, Robert Moszczynski. All rights reserved.

    Redistribution and use in source and binary forms, with or without
    modification, are permitted provided that the following conditions are met:

    1. Redistributions of source code must retain the above copyright notice, this
       list of conditions and the following disclaimer.
    2. Redistributions in binary form must reproduce the above copyright notice,
       this list of conditions and the following disclaimer in the documentation
       and/or other materials provided with the distribution.

    THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
    ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
    WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
    DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR
    ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
    (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
    LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
    ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
    (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
    SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.


-----
Robert Moszczynski [GitHub](https://github.com/rmoszczynski)
