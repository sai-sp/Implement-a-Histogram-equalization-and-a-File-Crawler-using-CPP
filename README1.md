Implement a File Crawler with C++ from scratch

Crawler:
      A crawler is a program that visits Web sites (here it is locally used) and reads their pages and other information in order to create entries for a search engine index.
      
Crawling is performed by anywhere from one to potentially hundreds of threads, each of which loops through the logical cycle in. 
These threads may be run in a single process, or be partitioned amongst multiple processes running at different nodes of a distributed system. 
We begin by assuming that the URL frontier is in place and non-empty and defer our description of the implementation of the URL frontier to Section. 
We follow the progress of a single URL through the cycle of being fetched, passing through various checks and filters, then finally (for continuous crawling) being returned to the URL frontier.

This Entire Program is for Web Crawler,for Crawl local files, files from sytem is used.

#include <CkSpider.h>
#include <CkStringArray.h>

void sample(void)
    {
    CkSpider spider;

    CkStringArray seenDomains;
    CkStringArray seedUrls;

    seenDomains.put_Unique(true);
    seedUrls.put_Unique(true);

    //  You will need to change the start URL to something else...
    seedUrls.Append("http://something.whateverYouWant.com/");

    //  Set outbound URL exclude patterns
    //  URLs matching any of these patterns will not be added to the
    //  collection of outbound links.
    spider.AddAvoidOutboundLinkPattern("*?id=*");
    spider.AddAvoidOutboundLinkPattern("*.mypages.*");
    spider.AddAvoidOutboundLinkPattern("*.personal.*");
    spider.AddAvoidOutboundLinkPattern("*.comcast.*");
    spider.AddAvoidOutboundLinkPattern("*.aol.*");
    spider.AddAvoidOutboundLinkPattern("*~*");

    //  Use a cache so we don't have to re-fetch URLs previously fetched.
    spider.put_CacheDir("c:/spiderCache/");
    spider.put_FetchFromCache(true);
    spider.put_UpdateCache(true);

    while (seedUrls.get_Count() > 0) {

        const char *url = seedUrls.pop();
        spider.Initialize(url);

        //  Spider 5 URLs of this domain.
        //  but first, save the base domain in seenDomains
        const char *domain = spider.getUrlDomain(url);
        seenDomains.Append(spider.getBaseDomain(domain));

        int i;
        bool success;
        for (i = 0; i <= 4; i++) {
            success = spider.CrawlNext();
            if (success == true) {

                //  Display the URL we just crawled.
                std::cout << spider.lastUrl() << "\r\n";

                //  If the last URL was retrieved from cache,
                //  we won't wait.  Otherwise we'll wait 1 second
                //  before fetching the next URL.
                if (spider.get_LastFromCache() != true) {
                    spider.SleepMs(1000);
                }

            }
            else {
                //  cause the loop to exit..
                i = 999;
            }

        }

        //  Add the outbound links to seedUrls, except
        //  for the domains we've already seen.
        for (i = 0; i <= spider.get_NumOutboundLinks() - 1; i++) {

            url = spider.getOutboundLink(i);
            const char *domain = spider.getUrlDomain(url);
            const char *baseDomain = spider.getBaseDomain(domain);
            if (seenDomains.Contains(baseDomain) == false) {
                //  Don't let our list of seedUrls grow too large.
                if (seedUrls.get_Count() < 1000) {
                    seedUrls.Append(url);
                }

            }

        }

    }


    }
