# HEY THERE
This is the PHP version of Kibana. Development has shifted to the Ruby version. 
It has all the same great features, but its even easier to setup, use and hack
on. Find it here: https://github.com/rashidkpc/Kibana/tree/kibana-ruby

To clone the kibana-ruby branch use:   
git clone --branch=kibana-ruby git@github.com:rashidkpc/Kibana.git

IMPORTANT: The master branch will soon be replaced with the kibana-ruby branch

# Kibana
A brower based log analysis tool for Logstash and ElasticSearch  
Copyright 2011 Rashid Khan <rashidkpc #logstash irc.freenode.net>  

## Installation
This assumes you already have a functioning logstash/elasticsearch
infrastructure, perhaps with some filters to define fields. Config
of logstash and/or elasticsearch is outside of the scope of this file

Requirements:  
logstash >= 1.1.0  
elasticsearch >= 0.18.0  
php >= 5.2  
php5-curl  

Other than that, it works just like any other PHP application. Untar 
and go at it.   

Edit config.php to set your elasticsearch server.   

## Notes
Q: Why is there no last button?  
A: ElasticSearch isn't so hot at going to the last result of a many million 
result query.  

Q: Why is this in PHP instead of Java, Ruby, etc?  
A: Because PHP is what I know. Its mostly javascript anyway. If you want it in 
something else, it shouldn't be too hard to port it to your language of choice.  

Q: Why do I have to set a limit on events to analyze  
A: Big result sets take a long time to retrieve from elasticsearch and parse out  

Q: Well then why don't you use the Elastic Search terms facet?  
A: I've found the terms facet to cause out of memory crashes with large result 
sets. I don't know a way to limit the amount of memory a facet may use. Until 
there's a way to run a facet and know for sure it  won't crash Elastic Search, 
I'm going to keep analysis features implemented in PHP. I'm open to other 
suggestions though. I suggest you be careful with the Statistics mode, its more
stable than terms, but can still bite you.  

Q: Why do come results not show up when I search for a string I know is in
the elasticsearch indexes?  
A: If you are searching analyzed fields, which is the default in ES for string
fields, remember that they are broken down into terms.  For instance, a search
for "test" will match records containing test@bleh.com, since @ is a term
boundary and is broken down into "test" and "bleh.com".  However, this will NOT
match records containing blah@test.com because "test.com" is the full term and
you are searching for an exact match.  You would need to use test* to match both
of these records.  Note you may also want to configure the ES analyze behavior
for certain fields if this is not the desired behavior.  Helpful References:  

  http://www.elasticsearch.org/guide/reference/mapping/core-types.html  
  http://www.elasticsearch.org/guide/reference/api/admin-indices-templates.html  

Q: Is there any way to prevent queries which perform full index scans of my
elasticsearch cluster?  
A: Yes.  Set the 'disable_fullscan' to 'true' and search terms beginning with
an elasticsearch wildcard (* or ?) will be stripped of the wildcard.  

Q: Where can I get some help with this?  
A: Find me on Freenode - rashidkpc in #logstash    

Q: How do I build an RPM for Kibana?  
A: On a RHEL 5/6-compatible system:  
- grab a Kibana tgz from https://github.com/rashidkpc/Kibana/tags  
- rename the file to kibana-<version>.tar.gz where <version> is in the form
  major.minor.patch (e.g. rashidkpc-Kibana-v0.1.5-0-g9753518.tar.gz becomes
  kibana-0.1.5.tar.gz)  
- generate the RPM with: rpmbuild -tb kibana-<version>.tar.gz  
