
__Part 1 exploring neighborhoods__


```python
!conda install -c conda-forge geopy --yes
!conda install -c conda-forge folium=0.5.0 --yes
from bs4 import BeautifulSoup
import requests
import pandas as pd
pd.set_option('display.max_columns', None)
pd.set_option('display.max_rows', None)
import numpy as np
from geopy.geocoders import Nominatim
import matplotlib.cm as cm
import matplotlib.colors as colors
from sklearn.cluster import KMeans
import folium
```

    Solving environment: done
    
    ## Package Plan ##
    
      environment location: /opt/conda/envs/DSX-Python35
    
      added / updated specs: 
        - geopy
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        geographiclib-1.49         |             py_0          32 KB  conda-forge
        openssl-1.0.2r             |       h14c3975_0         3.1 MB  conda-forge
        certifi-2018.8.24          |        py35_1001         139 KB  conda-forge
        ca-certificates-2019.3.9   |       hecc5488_0         146 KB  conda-forge
        geopy-1.20.0               |             py_0          57 KB  conda-forge
        ------------------------------------------------------------
                                               Total:         3.5 MB
    
    The following NEW packages will be INSTALLED:
    
        geographiclib:   1.49-py_0         conda-forge
        geopy:           1.20.0-py_0       conda-forge
    
    The following packages will be UPDATED:
    
        ca-certificates: 2019.1.23-0                   --> 2019.3.9-hecc5488_0 conda-forge
        certifi:         2018.8.24-py35_1              --> 2018.8.24-py35_1001 conda-forge
    
    The following packages will be DOWNGRADED:
    
        openssl:         1.0.2s-h7b6447c_0             --> 1.0.2r-h14c3975_0   conda-forge
    
    
    Downloading and Extracting Packages
    geographiclib-1.49   | 32 KB     | ##################################### | 100% 
    openssl-1.0.2r       | 3.1 MB    | ##################################### | 100% 
    certifi-2018.8.24    | 139 KB    | ##################################### | 100% 
    ca-certificates-2019 | 146 KB    | ##################################### | 100% 
    geopy-1.20.0         | 57 KB     | ##################################### | 100% 
    Preparing transaction: done
    Verifying transaction: done
    Executing transaction: done
    Solving environment: done
    
    ## Package Plan ##
    
      environment location: /opt/conda/envs/DSX-Python35
    
      added / updated specs: 
        - folium=0.5.0
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        altair-2.2.2               |           py35_1         462 KB  conda-forge
        vincent-0.4.4              |             py_1          28 KB  conda-forge
        folium-0.5.0               |             py_0          45 KB  conda-forge
        branca-0.3.1               |             py_0          25 KB  conda-forge
        ------------------------------------------------------------
                                               Total:         560 KB
    
    The following NEW packages will be INSTALLED:
    
        altair:  2.2.2-py35_1 conda-forge
        branca:  0.3.1-py_0   conda-forge
        folium:  0.5.0-py_0   conda-forge
        vincent: 0.4.4-py_1   conda-forge
    
    
    Downloading and Extracting Packages
    altair-2.2.2         | 462 KB    | ##################################### | 100% 
    vincent-0.4.4        | 28 KB     | ##################################### | 100% 
    folium-0.5.0         | 45 KB     | ##################################### | 100% 
    branca-0.3.1         | 25 KB     | ##################################### | 100% 
    Preparing transaction: done
    Verifying transaction: done
    Executing transaction: done


__Scraping__


```python
source = requests.get('https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M')
```


```python
soup = BeautifulSoup(source.content, 'html.parser')
print(soup.prettify())
```

    <!DOCTYPE html>
    <html class="client-nojs" dir="ltr" lang="en">
     <head>
      <meta charset="utf-8"/>
      <title>
       List of postal codes of Canada: M - Wikipedia
      </title>
      <script>
       document.documentElement.className=document.documentElement.className.replace(/(^|\s)client-nojs(\s|$)/,"$1client-js$2");RLCONF={"wgCanonicalNamespace":"","wgCanonicalSpecialPageName":!1,"wgNamespaceNumber":0,"wgPageName":"List_of_postal_codes_of_Canada:_M","wgTitle":"List of postal codes of Canada: M","wgCurRevisionId":900271985,"wgRevisionId":900271985,"wgArticleId":539066,"wgIsArticle":!0,"wgIsRedirect":!1,"wgAction":"view","wgUserName":null,"wgUserGroups":["*"],"wgCategories":["Communications in Ontario","Postal codes in Canada","Toronto","Ontario-related lists"],"wgBreakFrames":!1,"wgPageContentLanguage":"en","wgPageContentModel":"wikitext","wgSeparatorTransformTable":["",""],"wgDigitTransformTable":["",""],"wgDefaultDateFormat":"dmy","wgMonthNames":["","January","February","March","April","May","June","July","August","September","October","November","December"],"wgMonthNamesShort":["","Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"],
    "wgRelevantPageName":"List_of_postal_codes_of_Canada:_M","wgRelevantArticleId":539066,"wgRequestId":"XPjqFQpAIC4AAKz40vQAAABK","wgCSPNonce":!1,"wgIsProbablyEditable":!0,"wgRelevantPageIsProbablyEditable":!0,"wgRestrictionEdit":[],"wgRestrictionMove":[],"wgMediaViewerOnClick":!0,"wgMediaViewerEnabledByDefault":!0,"wgPopupsReferencePreviews":!1,"wgPopupsConflictsWithNavPopupGadget":!1,"wgVisualEditor":{"pageLanguageCode":"en","pageLanguageDir":"ltr","pageVariantFallbacks":"en"},"wgMFDisplayWikibaseDescriptions":{"search":!0,"nearby":!0,"watchlist":!0,"tagline":!1},"wgWMESchemaEditAttemptStepOversample":!1,"wgPoweredByHHVM":!0,"wgULSCurrentAutonym":"English","wgNoticeProject":"wikipedia","wgCentralNoticeCategoriesUsingLegacy":["Fundraising","fundraising"],"wgWikibaseItemId":"Q3248240","wgCentralAuthMobileDomain":!1,"wgEditSubmitButtonLabelPublish":!0};RLSTATE={"ext.gadget.charinsert-styles":"ready","ext.globalCssJs.user.styles":"ready",
    "ext.globalCssJs.site.styles":"ready","site.styles":"ready","noscript":"ready","user.styles":"ready","ext.globalCssJs.user":"ready","ext.globalCssJs.site":"ready","user":"ready","user.options":"ready","user.tokens":"loading","ext.cite.styles":"ready","mediawiki.legacy.shared":"ready","mediawiki.legacy.commonPrint":"ready","jquery.tablesorter.styles":"ready","wikibase.client.init":"ready","ext.visualEditor.desktopArticleTarget.noscript":"ready","ext.uls.interlanguage":"ready","ext.wikimediaBadges":"ready","ext.3d.styles":"ready","mediawiki.skinning.interface":"ready","skins.vector.styles":"ready"};RLPAGEMODULES=["ext.cite.ux-enhancements","site","mediawiki.page.startup","mediawiki.page.ready","jquery.tablesorter","mediawiki.searchSuggest","ext.gadget.teahouse","ext.gadget.ReferenceTooltips","ext.gadget.watchlist-notice","ext.gadget.DRN-wizard","ext.gadget.charinsert","ext.gadget.refToolbar","ext.gadget.extra-toolbar-buttons","ext.gadget.switcher","ext.centralauth.centralautologin",
    "mmv.head","mmv.bootstrap.autostart","ext.popups","ext.visualEditor.desktopArticleTarget.init","ext.visualEditor.targetLoader","ext.eventLogging","ext.wikimediaEvents","ext.navigationTiming","ext.uls.compactlinks","ext.uls.interface","ext.quicksurveys.init","ext.centralNotice.geoIP","ext.centralNotice.startUp","skins.vector.js"];
      </script>
      <script>
       (RLQ=window.RLQ||[]).push(function(){mw.loader.implement("user.tokens@0tffind",function($,jQuery,require,module){/*@nomin*/mw.user.tokens.set({"editToken":"+\\","patrolToken":"+\\","watchToken":"+\\","csrfToken":"+\\"});
    });});
      </script>
      <link href="/w/load.php?lang=en&amp;modules=ext.3d.styles%7Cext.cite.styles%7Cext.uls.interlanguage%7Cext.visualEditor.desktopArticleTarget.noscript%7Cext.wikimediaBadges%7Cjquery.tablesorter.styles%7Cmediawiki.legacy.commonPrint%2Cshared%7Cmediawiki.skinning.interface%7Cskins.vector.styles%7Cwikibase.client.init&amp;only=styles&amp;skin=vector" rel="stylesheet"/>
      <script async="" src="/w/load.php?lang=en&amp;modules=startup&amp;only=scripts&amp;skin=vector">
      </script>
      <meta content="" name="ResourceLoaderDynamicStyles"/>
      <link href="/w/load.php?lang=en&amp;modules=ext.gadget.charinsert-styles&amp;only=styles&amp;skin=vector" rel="stylesheet"/>
      <link href="/w/load.php?lang=en&amp;modules=site.styles&amp;only=styles&amp;skin=vector" rel="stylesheet"/>
      <meta content="MediaWiki 1.34.0-wmf.7" name="generator"/>
      <meta content="origin" name="referrer"/>
      <meta content="origin-when-crossorigin" name="referrer"/>
      <meta content="origin-when-cross-origin" name="referrer"/>
      <link href="android-app://org.wikipedia/http/en.m.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M" rel="alternate"/>
      <link href="/w/index.php?title=List_of_postal_codes_of_Canada:_M&amp;action=edit" rel="alternate" title="Edit this page" type="application/x-wiki"/>
      <link href="/w/index.php?title=List_of_postal_codes_of_Canada:_M&amp;action=edit" rel="edit" title="Edit this page"/>
      <link href="/static/apple-touch/wikipedia.png" rel="apple-touch-icon"/>
      <link href="/static/favicon/wikipedia.ico" rel="shortcut icon"/>
      <link href="/w/opensearch_desc.php" rel="search" title="Wikipedia (en)" type="application/opensearchdescription+xml"/>
      <link href="//en.wikipedia.org/w/api.php?action=rsd" rel="EditURI" type="application/rsd+xml"/>
      <link href="//creativecommons.org/licenses/by-sa/3.0/" rel="license"/>
      <link href="https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M" rel="canonical"/>
      <link href="//login.wikimedia.org" rel="dns-prefetch"/>
      <link href="//meta.wikimedia.org" rel="dns-prefetch"/>
      <!--[if lt IE 9]><script src="/w/load.php?lang=qqx&amp;modules=html5shiv&amp;only=scripts&amp;skin=fallback&amp;sync=1"></script><![endif]-->
     </head>
     <body class="mediawiki ltr sitedir-ltr mw-hide-empty-elt ns-0 ns-subject mw-editable page-List_of_postal_codes_of_Canada_M rootpage-List_of_postal_codes_of_Canada_M skin-vector action-view">
      <div class="noprint" id="mw-page-base">
      </div>
      <div class="noprint" id="mw-head-base">
      </div>
      <div class="mw-body" id="content" role="main">
       <a id="top">
       </a>
       <div class="mw-body-content" id="siteNotice">
        <!-- CentralNotice -->
       </div>
       <div class="mw-indicators mw-body-content">
       </div>
       <h1 class="firstHeading" id="firstHeading" lang="en">
        List of postal codes of Canada: M
       </h1>
       <div class="mw-body-content" id="bodyContent">
        <div class="noprint" id="siteSub">
         From Wikipedia, the free encyclopedia
        </div>
        <div id="contentSub">
        </div>
        <div id="jump-to-nav">
        </div>
        <a class="mw-jump-link" href="#mw-head">
         Jump to navigation
        </a>
        <a class="mw-jump-link" href="#p-search">
         Jump to search
        </a>
        <div class="mw-content-ltr" dir="ltr" id="mw-content-text" lang="en">
         <div class="mw-parser-output">
          <p>
           This is a list of
           <a href="/wiki/Postal_codes_in_Canada" title="Postal codes in Canada">
            postal codes in Canada
           </a>
           where the first letter is M. Postal codes beginning with M are located within the city of
           <a href="/wiki/Toronto" title="Toronto">
            Toronto
           </a>
           in the province of
           <a href="/wiki/Ontario" title="Ontario">
            Ontario
           </a>
           . Only the first three characters are listed, corresponding to the Forward Sortation Area.
          </p>
          <p>
           <a href="/wiki/Canada_Post" title="Canada Post">
            Canada Post
           </a>
           provides a free postal code look-up tool on its website,
           <sup class="reference" id="cite_ref-1">
            <a href="#cite_note-1">
             [1]
            </a>
           </sup>
           via its
           <a href="/wiki/Mobile_app" title="Mobile app">
            applications
           </a>
           for such
           <a class="mw-redirect" href="/wiki/Smartphones" title="Smartphones">
            smartphones
           </a>
           as the
           <a href="/wiki/IPhone" title="IPhone">
            iPhone
           </a>
           and
           <a href="/wiki/BlackBerry" title="BlackBerry">
            BlackBerry
           </a>
           ,
           <sup class="reference" id="cite_ref-2">
            <a href="#cite_note-2">
             [2]
            </a>
           </sup>
           and sells hard-copy directories and
           <a href="/wiki/CD-ROM" title="CD-ROM">
            CD-ROMs
           </a>
           . Many vendors also sell validation tools, which allow customers to properly match addresses and postal codes. Hard-copy directories can also be consulted in all post offices, and some libraries.
          </p>
          <h2>
           <span class="mw-headline" id="Toronto_-_FSAs">
            <a href="/wiki/Toronto" title="Toronto">
             Toronto
            </a>
            -
            <a href="/wiki/Postal_codes_in_Canada#Forward_sortation_areas" title="Postal codes in Canada">
             FSAs
            </a>
           </span>
           <span class="mw-editsection">
            <span class="mw-editsection-bracket">
             [
            </span>
            <a href="/w/index.php?title=List_of_postal_codes_of_Canada:_M&amp;action=edit&amp;section=1" title="Edit section: Toronto - FSAs">
             edit
            </a>
            <span class="mw-editsection-bracket">
             ]
            </span>
           </span>
          </h2>
          <p>
           Note: There are no rural FSAs in Toronto, hence no postal codes start with M0.
          </p>
          <table class="wikitable sortable">
           <tbody>
            <tr>
             <th>
              Postcode
             </th>
             <th>
              Borough
             </th>
             <th>
              Neighbourhood
             </th>
            </tr>
            <tr>
             <td>
              M1A
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M2A
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M3A
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a href="/wiki/Parkwoods" title="Parkwoods">
               Parkwoods
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M4A
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a href="/wiki/Victoria_Village" title="Victoria Village">
               Victoria Village
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5A
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Harbourfront_(Toronto)" title="Harbourfront (Toronto)">
               Harbourfront
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5A
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Regent_Park" title="Regent Park">
               Regent Park
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6A
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a href="/wiki/Lawrence_Heights" title="Lawrence Heights">
               Lawrence Heights
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6A
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a href="/wiki/Lawrence_Manor" title="Lawrence Manor">
               Lawrence Manor
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M7A
             </td>
             <td>
              <a href="/wiki/Queen%27s_Park_(Toronto)" title="Queen's Park (Toronto)">
               Queen's Park
              </a>
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M8A
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M9A
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Islington_Avenue" title="Islington Avenue">
               Islington Avenue
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1B
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Rouge,_Toronto" title="Rouge, Toronto">
               Rouge
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1B
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Malvern,_Toronto" title="Malvern, Toronto">
               Malvern
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M2B
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M3B
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              Don Mills North
             </td>
            </tr>
            <tr>
             <td>
              M4B
             </td>
             <td>
              <a href="/wiki/East_York" title="East York">
               East York
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Woodbine_Gardens" title="Woodbine Gardens">
               Woodbine Gardens
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M4B
             </td>
             <td>
              <a href="/wiki/East_York" title="East York">
               East York
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Parkview_Hill" title="Parkview Hill">
               Parkview Hill
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5B
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              Ryerson
             </td>
            </tr>
            <tr>
             <td>
              M5B
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              Garden District
             </td>
            </tr>
            <tr>
             <td>
              M6B
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              Glencairn
             </td>
            </tr>
            <tr>
             <td>
              M7B
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M8B
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M9B
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              Cloverdale
             </td>
            </tr>
            <tr>
             <td>
              M9B
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Islington,_Toronto" title="Islington, Toronto">
               Islington
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M9B
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              Martin Grove
             </td>
            </tr>
            <tr>
             <td>
              M9B
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a href="/wiki/Princess_Gardens" title="Princess Gardens">
               Princess Gardens
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M9B
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/West_Deane_Park" title="West Deane Park">
               West Deane Park
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1C
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Highland_Creek_(Toronto)" title="Highland Creek (Toronto)">
               Highland Creek
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1C
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Rouge_Hill" title="Rouge Hill">
               Rouge Hill
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1C
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Port_Union,_Toronto" title="Port Union, Toronto">
               Port Union
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M2C
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M3C
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a href="/wiki/Flemingdon_Park" title="Flemingdon Park">
               Flemingdon Park
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M3C
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              Don Mills South
             </td>
            </tr>
            <tr>
             <td>
              M4C
             </td>
             <td>
              <a href="/wiki/East_York" title="East York">
               East York
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Woodbine_Heights" title="Woodbine Heights">
               Woodbine Heights
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5C
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/St._James_Town" title="St. James Town">
               St. James Town
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6C
             </td>
             <td>
              York
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Humewood-Cedarvale" title="Humewood-Cedarvale">
               Humewood-Cedarvale
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M7C
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M8C
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M9C
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              Bloordale Gardens
             </td>
            </tr>
            <tr>
             <td>
              M9C
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              Eringate
             </td>
            </tr>
            <tr>
             <td>
              M9C
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a href="/wiki/Markland_Wood" title="Markland Wood">
               Markland Wood
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M9C
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              Old Burnhamthorpe
             </td>
            </tr>
            <tr>
             <td>
              M1E
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              Guildwood
             </td>
            </tr>
            <tr>
             <td>
              M1E
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Morningside,_Toronto" title="Morningside, Toronto">
               Morningside
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1E
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/West_Hill,_Toronto" title="West Hill, Toronto">
               West Hill
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M2E
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M3E
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M4E
             </td>
             <td>
              <a href="/wiki/East_Toronto" title="East Toronto">
               East Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/The_Beaches" title="The Beaches">
               The Beaches
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5E
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Berczy_Park" title="Berczy Park">
               Berczy Park
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6E
             </td>
             <td>
              York
             </td>
             <td>
              Caledonia-Fairbanks
             </td>
            </tr>
            <tr>
             <td>
              M7E
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M8E
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M9E
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M1G
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Woburn,_Toronto" title="Woburn, Toronto">
               Woburn
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M2G
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M3G
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M4G
             </td>
             <td>
              <a href="/wiki/East_York" title="East York">
               East York
              </a>
             </td>
             <td>
              <a href="/wiki/Leaside" title="Leaside">
               Leaside
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5G
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              Central Bay Street
             </td>
            </tr>
            <tr>
             <td>
              M6G
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              Christie
             </td>
            </tr>
            <tr>
             <td>
              M7G
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M8G
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M9G
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M1H
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              Cedarbrae
             </td>
            </tr>
            <tr>
             <td>
              M2H
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a href="/wiki/Hillcrest_Village" title="Hillcrest Village">
               Hillcrest Village
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M3H
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a href="/wiki/Bathurst_Manor" title="Bathurst Manor">
               Bathurst Manor
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M3H
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              Downsview North
             </td>
            </tr>
            <tr>
             <td>
              M3H
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Wilson_Heights,_Toronto" title="Wilson Heights, Toronto">
               Wilson Heights
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M4H
             </td>
             <td>
              <a href="/wiki/East_York" title="East York">
               East York
              </a>
             </td>
             <td>
              <a href="/wiki/Thorncliffe_Park" title="Thorncliffe Park">
               Thorncliffe Park
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5H
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              Adelaide
             </td>
            </tr>
            <tr>
             <td>
              M5H
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              King
             </td>
            </tr>
            <tr>
             <td>
              M5H
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              Richmond
             </td>
            </tr>
            <tr>
             <td>
              M6H
             </td>
             <td>
              <a href="/wiki/West_Toronto" title="West Toronto">
               West Toronto
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Dovercourt_Village" title="Dovercourt Village">
               Dovercourt Village
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6H
             </td>
             <td>
              <a href="/wiki/West_Toronto" title="West Toronto">
               West Toronto
              </a>
             </td>
             <td>
              Dufferin
             </td>
            </tr>
            <tr>
             <td>
              M7H
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M8H
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M9H
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M1J
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Scarborough_Village" title="Scarborough Village">
               Scarborough Village
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M2J
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              Fairview
             </td>
            </tr>
            <tr>
             <td>
              M2J
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a href="/wiki/Henry_Farm" title="Henry Farm">
               Henry Farm
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M2J
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              Oriole
             </td>
            </tr>
            <tr>
             <td>
              M3J
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Northwood_Park" title="Northwood Park">
               Northwood Park
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M3J
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a href="/wiki/York_University" title="York University">
               York University
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M4J
             </td>
             <td>
              <a href="/wiki/East_York" title="East York">
               East York
              </a>
             </td>
             <td>
              <a href="/wiki/East_Toronto" title="East Toronto">
               East Toronto
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5J
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              Harbourfront East
             </td>
            </tr>
            <tr>
             <td>
              M5J
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Toronto_Islands" title="Toronto Islands">
               Toronto Islands
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5J
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Union_Station_(Toronto)" title="Union Station (Toronto)">
               Union Station
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6J
             </td>
             <td>
              <a href="/wiki/West_Toronto" title="West Toronto">
               West Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Little_Portugal,_Toronto" title="Little Portugal, Toronto">
               Little Portugal
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6J
             </td>
             <td>
              <a href="/wiki/West_Toronto" title="West Toronto">
               West Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Trinity%E2%80%93Bellwoods" title="Trinity–Bellwoods">
               Trinity
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M7J
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M8J
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M9J
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M1K
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              East Birchmount Park
             </td>
            </tr>
            <tr>
             <td>
              M1K
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Ionview" title="Ionview">
               Ionview
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1K
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Kennedy_Park,_Toronto" title="Kennedy Park, Toronto">
               Kennedy Park
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M2K
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a href="/wiki/Bayview_Village" title="Bayview Village">
               Bayview Village
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M3K
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a href="/wiki/CFB_Toronto" title="CFB Toronto">
               CFB Toronto
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M3K
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              Downsview East
             </td>
            </tr>
            <tr>
             <td>
              M4K
             </td>
             <td>
              <a href="/wiki/East_Toronto" title="East Toronto">
               East Toronto
              </a>
             </td>
             <td>
              The Danforth West
             </td>
            </tr>
            <tr>
             <td>
              M4K
             </td>
             <td>
              <a href="/wiki/East_Toronto" title="East Toronto">
               East Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Riverdale,_Toronto" title="Riverdale, Toronto">
               Riverdale
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5K
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Design_Exchange" title="Design Exchange">
               Design Exchange
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5K
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Toronto_Dominion_Centre" title="Toronto Dominion Centre">
               Toronto Dominion Centre
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6K
             </td>
             <td>
              <a href="/wiki/West_Toronto" title="West Toronto">
               West Toronto
              </a>
             </td>
             <td>
              Brockton
             </td>
            </tr>
            <tr>
             <td>
              M6K
             </td>
             <td>
              <a href="/wiki/West_Toronto" title="West Toronto">
               West Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Exhibition_Place" title="Exhibition Place">
               Exhibition Place
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6K
             </td>
             <td>
              <a href="/wiki/West_Toronto" title="West Toronto">
               West Toronto
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Parkdale_Village" title="Parkdale Village">
               Parkdale Village
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M7K
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M8K
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M9K
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M1L
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Clairlea" title="Clairlea">
               Clairlea
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1L
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Golden_Mile,_Toronto" title="Golden Mile, Toronto">
               Golden Mile
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1L
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Oakridge,_Toronto" title="Oakridge, Toronto">
               Oakridge
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M2L
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              Silver Hills
             </td>
            </tr>
            <tr>
             <td>
              M2L
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a href="/wiki/York_Mills" title="York Mills">
               York Mills
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M3L
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a href="/wiki/Downsview" title="Downsview">
               Downsview West
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M4L
             </td>
             <td>
              <a href="/wiki/East_Toronto" title="East Toronto">
               East Toronto
              </a>
             </td>
             <td>
              The Beaches West
             </td>
            </tr>
            <tr>
             <td>
              M4L
             </td>
             <td>
              <a href="/wiki/East_Toronto" title="East Toronto">
               East Toronto
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/India_Bazaar" title="India Bazaar">
               India Bazaar
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5L
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Commerce_Court" title="Commerce Court">
               Commerce Court
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5L
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              Victoria Hotel
             </td>
            </tr>
            <tr>
             <td>
              M6L
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Downsview,_Toronto" title="Downsview, Toronto">
               Downsview
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6L
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              North Park
             </td>
            </tr>
            <tr>
             <td>
              M6L
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              Upwood Park
             </td>
            </tr>
            <tr>
             <td>
              M7L
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M8L
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M9L
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a href="/wiki/Humber_Summit" title="Humber Summit">
               Humber Summit
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1M
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Cliffcrest" title="Cliffcrest">
               Cliffcrest
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1M
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Cliffside,_Toronto" title="Cliffside, Toronto">
               Cliffside
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1M
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              Scarborough Village West
             </td>
            </tr>
            <tr>
             <td>
              M2M
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a href="/wiki/Newtonbrook" title="Newtonbrook">
               Newtonbrook
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M2M
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a href="/wiki/Willowdale,_Toronto" title="Willowdale, Toronto">
               Willowdale
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M3M
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              Downsview Central
             </td>
            </tr>
            <tr>
             <td>
              M4M
             </td>
             <td>
              <a href="/wiki/East_Toronto" title="East Toronto">
               East Toronto
              </a>
             </td>
             <td>
              Studio District
             </td>
            </tr>
            <tr>
             <td>
              M5M
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a href="/wiki/Bedford_Park,_Toronto" title="Bedford Park, Toronto">
               Bedford Park
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5M
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              Lawrence Manor East
             </td>
            </tr>
            <tr>
             <td>
              M6M
             </td>
             <td>
              <a href="/wiki/York,_Toronto" title="York, Toronto">
               York
              </a>
             </td>
             <td>
              Del Ray
             </td>
            </tr>
            <tr>
             <td>
              M6M
             </td>
             <td>
              <a href="/wiki/York,_Toronto" title="York, Toronto">
               York
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Keelesdale" title="Keelesdale">
               Keelesdale
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6M
             </td>
             <td>
              <a href="/wiki/York,_Toronto" title="York, Toronto">
               York
              </a>
             </td>
             <td>
              <a href="/wiki/Mount_Dennis" title="Mount Dennis">
               Mount Dennis
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6M
             </td>
             <td>
              <a href="/wiki/York,_Toronto" title="York, Toronto">
               York
              </a>
             </td>
             <td>
              <a href="/wiki/Silverthorn,_Toronto" title="Silverthorn, Toronto">
               Silverthorn
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M7M
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M8M
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M9M
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Emery,_Toronto" title="Emery, Toronto">
               Emery
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M9M
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Humberlea" title="Humberlea">
               Humberlea
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1N
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Birch_Cliff" title="Birch Cliff">
               Birch Cliff
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1N
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              Cliffside West
             </td>
            </tr>
            <tr>
             <td>
              M2N
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              Willowdale South
             </td>
            </tr>
            <tr>
             <td>
              M3N
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              Downsview Northwest
             </td>
            </tr>
            <tr>
             <td>
              M4N
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Central_Toronto" title="Central Toronto">
               Central Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Lawrence_Park,_Toronto" title="Lawrence Park, Toronto">
               Lawrence Park
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5N
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Central_Toronto" title="Central Toronto">
               Central Toronto
              </a>
             </td>
             <td>
              Roselawn
             </td>
            </tr>
            <tr>
             <td>
              M6N
             </td>
             <td>
              <a href="/wiki/York,_Toronto" title="York, Toronto">
               York
              </a>
             </td>
             <td>
              The Junction North
             </td>
            </tr>
            <tr>
             <td>
              M6N
             </td>
             <td>
              <a href="/wiki/York,_Toronto" title="York, Toronto">
               York
              </a>
             </td>
             <td>
              Runnymede
             </td>
            </tr>
            <tr>
             <td>
              M7N
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M8N
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M9N
             </td>
             <td>
              <a href="/wiki/York,_Toronto" title="York, Toronto">
               York
              </a>
             </td>
             <td>
              <a href="/wiki/Weston,_Toronto" title="Weston, Toronto">
               Weston
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1P
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Dorset_Park" title="Dorset Park">
               Dorset Park
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1P
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Scarborough_Town_Centre" title="Scarborough Town Centre">
               Scarborough Town Centre
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1P
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Wexford_Heights" title="Wexford Heights">
               Wexford Heights
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M2P
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              York Mills West
             </td>
            </tr>
            <tr>
             <td>
              M3P
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M4P
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Central_Toronto" title="Central Toronto">
               Central Toronto
              </a>
             </td>
             <td>
              Davisville North
             </td>
            </tr>
            <tr>
             <td>
              M5P
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Central_Toronto" title="Central Toronto">
               Central Toronto
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Forest_Hill_North" title="Forest Hill North">
               Forest Hill North
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5P
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Central_Toronto" title="Central Toronto">
               Central Toronto
              </a>
             </td>
             <td>
              Forest Hill West
             </td>
            </tr>
            <tr>
             <td>
              M6P
             </td>
             <td>
              <a href="/wiki/West_Toronto" title="West Toronto">
               West Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/High_Park" title="High Park">
               High Park
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6P
             </td>
             <td>
              <a href="/wiki/West_Toronto" title="West Toronto">
               West Toronto
              </a>
             </td>
             <td>
              The Junction South
             </td>
            </tr>
            <tr>
             <td>
              M7P
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M8P
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M9P
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              Westmount
             </td>
            </tr>
            <tr>
             <td>
              M1R
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Maryvale,_Toronto" title="Maryvale, Toronto">
               Maryvale
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1R
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Wexford,_Toronto" title="Wexford, Toronto">
               Wexford
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M2R
             </td>
             <td>
              <a href="/wiki/North_York" title="North York">
               North York
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Willowdale_West" title="Willowdale West">
               Willowdale West
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M3R
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M4R
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Central_Toronto" title="Central Toronto">
               Central Toronto
              </a>
             </td>
             <td>
              North Toronto West
             </td>
            </tr>
            <tr>
             <td>
              M5R
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Central_Toronto" title="Central Toronto">
               Central Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/The_Annex" title="The Annex">
               The Annex
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5R
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Central_Toronto" title="Central Toronto">
               Central Toronto
              </a>
             </td>
             <td>
              North Midtown
             </td>
            </tr>
            <tr>
             <td>
              M5R
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Central_Toronto" title="Central Toronto">
               Central Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Yorkville,_Toronto" title="Yorkville, Toronto">
               Yorkville
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6R
             </td>
             <td>
              <a href="/wiki/West_Toronto" title="West Toronto">
               West Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Parkdale,_Toronto" title="Parkdale, Toronto">
               Parkdale
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6R
             </td>
             <td>
              <a href="/wiki/West_Toronto" title="West Toronto">
               West Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Roncesvalles,_Toronto" title="Roncesvalles, Toronto">
               Roncesvalles
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M7R
             </td>
             <td>
              Mississauga
             </td>
             <td>
              Canada Post Gateway Processing Centre
             </td>
            </tr>
            <tr>
             <td>
              M8R
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M9R
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a href="/wiki/Kingsview_Village" title="Kingsview Village">
               Kingsview Village
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M9R
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              Martin Grove Gardens
             </td>
            </tr>
            <tr>
             <td>
              M9R
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              Richview Gardens
             </td>
            </tr>
            <tr>
             <td>
              M9R
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              St. Phillips
             </td>
            </tr>
            <tr>
             <td>
              M1S
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Agincourt,_Toronto" title="Agincourt, Toronto">
               Agincourt
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M2S
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M3S
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M4S
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Central_Toronto" title="Central Toronto">
               Central Toronto
              </a>
             </td>
             <td>
              Davisville
             </td>
            </tr>
            <tr>
             <td>
              M5S
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              Harbord
             </td>
            </tr>
            <tr>
             <td>
              M5S
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/University_of_Toronto" title="University of Toronto">
               University of Toronto
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6S
             </td>
             <td>
              <a href="/wiki/West_Toronto" title="West Toronto">
               West Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Runnymede,_Toronto" title="Runnymede, Toronto">
               Runnymede
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6S
             </td>
             <td>
              <a href="/wiki/West_Toronto" title="West Toronto">
               West Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Swansea,_Toronto" title="Swansea, Toronto">
               Swansea
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M7S
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M8S
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M9S
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M1T
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              Clarks Corners
             </td>
            </tr>
            <tr>
             <td>
              M1T
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              Sullivan
             </td>
            </tr>
            <tr>
             <td>
              M1T
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Tam_O%27Shanter_%E2%80%93_Sullivan" title="Tam O'Shanter – Sullivan">
               Tam O'Shanter
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M2T
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M3T
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M4T
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Central_Toronto" title="Central Toronto">
               Central Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Moore_Park,_Toronto" title="Moore Park, Toronto">
               Moore Park
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M4T
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Central_Toronto" title="Central Toronto">
               Central Toronto
              </a>
             </td>
             <td>
              Summerhill East
             </td>
            </tr>
            <tr>
             <td>
              M5T
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Chinatown,_Toronto" title="Chinatown, Toronto">
               Chinatown
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5T
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Grange_Park_(Toronto)" title="Grange Park (Toronto)">
               Grange Park
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5T
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Kensington_Market" title="Kensington Market">
               Kensington Market
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6T
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M7T
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M8T
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M9T
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M1V
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Agincourt_North" title="Agincourt North">
               Agincourt North
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1V
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              L'Amoreaux East
             </td>
            </tr>
            <tr>
             <td>
              M1V
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a href="/wiki/Milliken,_Ontario" title="Milliken, Ontario">
               Milliken
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1V
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              Steeles East
             </td>
            </tr>
            <tr>
             <td>
              M2V
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M3V
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M4V
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Central_Toronto" title="Central Toronto">
               Central Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Deer_Park,_Toronto" title="Deer Park, Toronto">
               Deer Park
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M4V
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Central_Toronto" title="Central Toronto">
               Central Toronto
              </a>
             </td>
             <td>
              Forest Hill SE
             </td>
            </tr>
            <tr>
             <td>
              M4V
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Central_Toronto" title="Central Toronto">
               Central Toronto
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Rathnelly" title="Rathnelly">
               Rathnelly
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M4V
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Central_Toronto" title="Central Toronto">
               Central Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/South_Hill,_Toronto" title="South Hill, Toronto">
               South Hill
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M4V
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Central_Toronto" title="Central Toronto">
               Central Toronto
              </a>
             </td>
             <td>
              Summerhill West
             </td>
            </tr>
            <tr>
             <td>
              M5V
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/CN_Tower" title="CN Tower">
               CN Tower
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5V
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              Bathurst Quay
             </td>
            </tr>
            <tr>
             <td>
              M5V
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              Island airport
             </td>
            </tr>
            <tr>
             <td>
              M5V
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              Harbourfront West
             </td>
            </tr>
            <tr>
             <td>
              M5V
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/King_and_Spadina" title="King and Spadina">
               King and Spadina
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5V
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Railway_Lands" title="Railway Lands">
               Railway Lands
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5V
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/South_Niagara" title="South Niagara">
               South Niagara
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6V
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M7V
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M8V
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              Humber Bay Shores
             </td>
            </tr>
            <tr>
             <td>
              M8V
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              Mimico South
             </td>
            </tr>
            <tr>
             <td>
              M8V
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a href="/wiki/New_Toronto" title="New Toronto">
               New Toronto
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M9V
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              Albion Gardens
             </td>
            </tr>
            <tr>
             <td>
              M9V
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Beaumond_Heights" title="Beaumond Heights">
               Beaumond Heights
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M9V
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              Humbergate
             </td>
            </tr>
            <tr>
             <td>
              M9V
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Mount_Olive-Silverstone-Jamestown" title="Mount Olive-Silverstone-Jamestown">
               Jamestown
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M9V
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Mount_Olive-Silverstone-Jamestown" title="Mount Olive-Silverstone-Jamestown">
               Mount Olive
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M9V
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Silverstone,_Toronto" title="Silverstone, Toronto">
               Silverstone
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M9V
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/South_Steeles" title="South Steeles">
               South Steeles
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M9V
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a href="/wiki/Thistletown" title="Thistletown">
               Thistletown
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M1W
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              L'Amoreaux West
             </td>
            </tr>
            <tr>
             <td>
              M2W
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M3W
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M4W
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Rosedale,_Toronto" title="Rosedale, Toronto">
               Rosedale
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5W
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              Stn A PO Boxes 25 The Esplanade
             </td>
            </tr>
            <tr>
             <td>
              M6W
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M7W
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M8W
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a href="/wiki/Alderwood,_Toronto" title="Alderwood, Toronto">
               Alderwood
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M8W
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a href="/wiki/Long_Branch,_Toronto" title="Long Branch, Toronto">
               Long Branch
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M9W
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              Northwest
             </td>
            </tr>
            <tr>
             <td>
              M1X
             </td>
             <td>
              <a href="/wiki/Scarborough,_Toronto" title="Scarborough, Toronto">
               Scarborough
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Upper_Rouge" title="Upper Rouge">
               Upper Rouge
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M2X
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M3X
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M4X
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Cabbagetown,_Toronto" title="Cabbagetown, Toronto">
               Cabbagetown
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M4X
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/St._James_Town" title="St. James Town">
               St. James Town
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5X
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/First_Canadian_Place" title="First Canadian Place">
               First Canadian Place
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5X
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Underground_city" title="Underground city">
               Underground city
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M6X
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M7X
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M8X
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a href="/wiki/The_Kingsway" title="The Kingsway">
               The Kingsway
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M8X
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              Montgomery Road
             </td>
            </tr>
            <tr>
             <td>
              M8X
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              Old Mill North
             </td>
            </tr>
            <tr>
             <td>
              M9X
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M1Y
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M2Y
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M3Y
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M4Y
             </td>
             <td>
              <a href="/wiki/Downtown_Toronto" title="Downtown Toronto">
               Downtown Toronto
              </a>
             </td>
             <td>
              <a href="/wiki/Church_and_Wellesley" title="Church and Wellesley">
               Church and Wellesley
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M5Y
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M6Y
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M7Y
             </td>
             <td>
              <a href="/wiki/East_Toronto" title="East Toronto">
               East Toronto
              </a>
             </td>
             <td>
              Business Reply Mail Processing Centre 969 Eastern
             </td>
            </tr>
            <tr>
             <td>
              M8Y
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a href="/wiki/Humber_Bay" title="Humber Bay">
               Humber Bay
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M8Y
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              King's Mill Park
             </td>
            </tr>
            <tr>
             <td>
              M8Y
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              Kingsway Park South East
             </td>
            </tr>
            <tr>
             <td>
              M8Y
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a href="/wiki/Mimico" title="Mimico">
               Mimico NE
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M8Y
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a href="/wiki/Old_Mill,_Toronto" title="Old Mill, Toronto">
               Old Mill South
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M8Y
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a href="/wiki/The_Queensway" title="The Queensway">
               The Queensway East
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M8Y
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Fairmont_Royal_York_Hotel" title="Fairmont Royal York Hotel">
               Royal York South East
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M8Y
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a class="mw-redirect" href="/wiki/Sunnylea" title="Sunnylea">
               Sunnylea
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M9Y
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M1Z
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M2Z
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M3Z
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M4Z
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M5Z
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M6Z
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M7Z
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
            <tr>
             <td>
              M8Z
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              Kingsway Park South West
             </td>
            </tr>
            <tr>
             <td>
              M8Z
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a href="/wiki/Mimico" title="Mimico">
               Mimico NW
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M8Z
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              <a href="/wiki/The_Queensway" title="The Queensway">
               The Queensway West
              </a>
             </td>
            </tr>
            <tr>
             <td>
              M8Z
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              Royal York South West
             </td>
            </tr>
            <tr>
             <td>
              M8Z
             </td>
             <td>
              <a href="/wiki/Etobicoke" title="Etobicoke">
               Etobicoke
              </a>
             </td>
             <td>
              South of Bloor
             </td>
            </tr>
            <tr>
             <td>
              M9Z
             </td>
             <td>
              Not assigned
             </td>
             <td>
              Not assigned
             </td>
            </tr>
           </tbody>
          </table>
          <div>
           <table class="multicol" role="presentation" style="border-collapse: collapse; padding: 0; border: 0; background:transparent; width:100%;">
           </table>
           <h2>
            <span id="Most_populated_FSAs.5B3.5D">
            </span>
            <span class="mw-headline" id="Most_populated_FSAs[3]">
             Most populated FSAs
             <sup class="reference" id="cite_ref-statcan_3-0">
              <a href="#cite_note-statcan-3">
               [3]
              </a>
             </sup>
            </span>
            <span class="mw-editsection">
             <span class="mw-editsection-bracket">
              [
             </span>
             <a href="/w/index.php?title=List_of_postal_codes_of_Canada:_M&amp;action=edit&amp;section=2" title="Edit section: Most populated FSAs[3]">
              edit
             </a>
             <span class="mw-editsection-bracket">
              ]
             </span>
            </span>
           </h2>
           <ol>
            <li>
             M1B, 65,129
            </li>
            <li>
             M2N, 60,124
            </li>
            <li>
             M1V, 55,250
            </li>
            <li>
             M9V, 55,159
            </li>
            <li>
             M2J, 54,391
            </li>
           </ol>
           <p>
           </p>
           <table cellpadding="2" cellspacing="0" rules="all" style="width:100%; border-collapse:collapse; border:1px solid #ccc;">
            <tbody>
             <tr>
              <td>
              </td>
             </tr>
            </tbody>
           </table>
          </div>
          <p>
          </p>
          <h2>
           <span id="Least_populated_FSAs.5B3.5D">
           </span>
           <span class="mw-headline" id="Least_populated_FSAs[3]">
            Least populated FSAs
            <sup class="reference" id="cite_ref-statcan_3-1">
             <a href="#cite_note-statcan-3">
              [3]
             </a>
            </sup>
           </span>
           <span class="mw-editsection">
            <span class="mw-editsection-bracket">
             [
            </span>
            <a href="/w/index.php?title=List_of_postal_codes_of_Canada:_M&amp;action=edit&amp;section=3" title="Edit section: Least populated FSAs[3]">
             edit
            </a>
            <span class="mw-editsection-bracket">
             ]
            </span>
           </span>
          </h2>
          <ol>
           <li>
            M5K, 5
           </li>
           <li>
            M5L, 5
           </li>
           <li>
            M5W, 5
           </li>
           <li>
            M5X, 5
           </li>
           <li>
            M7A, 5
           </li>
          </ol>
          <p>
          </p>
          <h2>
           <span class="mw-headline" id="References">
            References
           </span>
           <span class="mw-editsection">
            <span class="mw-editsection-bracket">
             [
            </span>
            <a href="/w/index.php?title=List_of_postal_codes_of_Canada:_M&amp;action=edit&amp;section=4" title="Edit section: References">
             edit
            </a>
            <span class="mw-editsection-bracket">
             ]
            </span>
           </span>
          </h2>
          <div class="mw-references-wrap">
           <ol class="references">
            <li id="cite_note-1">
             <span class="mw-cite-backlink">
              <b>
               <a href="#cite_ref-1">
                ^
               </a>
              </b>
             </span>
             <span class="reference-text">
              <cite class="citation web">
               Canada Post.
               <a class="external text" href="https://www.canadapost.ca/cpotools/apps/fpc/personal/findByCity?execution=e2s1" rel="nofollow">
                "Canada Post - Find a Postal Code"
               </a>
               <span class="reference-accessdate">
                . Retrieved
                <span class="nowrap">
                 9 November
                </span>
                2008
               </span>
               .
              </cite>
              <span class="Z3988" title="ctx_ver=Z39.88-2004&amp;rft_val_fmt=info%3Aofi%2Ffmt%3Akev%3Amtx%3Abook&amp;rft.genre=unknown&amp;rft.btitle=Canada+Post+-+Find+a+Postal+Code&amp;rft.au=Canada+Post&amp;rft_id=https%3A%2F%2Fwww.canadapost.ca%2Fcpotools%2Fapps%2Ffpc%2Fpersonal%2FfindByCity%3Fexecution%3De2s1&amp;rfr_id=info%3Asid%2Fen.wikipedia.org%3AList+of+postal+codes+of+Canada%3A+M">
              </span>
              <style data-mw-deduplicate="TemplateStyles:r886058088">
               .mw-parser-output cite.citation{font-style:inherit}.mw-parser-output .citation q{quotes:"\"""\"""'""'"}.mw-parser-output .citation .cs1-lock-free a{background:url("//upload.wikimedia.org/wikipedia/commons/thumb/6/65/Lock-green.svg/9px-Lock-green.svg.png")no-repeat;background-position:right .1em center}.mw-parser-output .citation .cs1-lock-limited a,.mw-parser-output .citation .cs1-lock-registration a{background:url("//upload.wikimedia.org/wikipedia/commons/thumb/d/d6/Lock-gray-alt-2.svg/9px-Lock-gray-alt-2.svg.png")no-repeat;background-position:right .1em center}.mw-parser-output .citation .cs1-lock-subscription a{background:url("//upload.wikimedia.org/wikipedia/commons/thumb/a/aa/Lock-red-alt-2.svg/9px-Lock-red-alt-2.svg.png")no-repeat;background-position:right .1em center}.mw-parser-output .cs1-subscription,.mw-parser-output .cs1-registration{color:#555}.mw-parser-output .cs1-subscription span,.mw-parser-output .cs1-registration span{border-bottom:1px dotted;cursor:help}.mw-parser-output .cs1-ws-icon a{background:url("//upload.wikimedia.org/wikipedia/commons/thumb/4/4c/Wikisource-logo.svg/12px-Wikisource-logo.svg.png")no-repeat;background-position:right .1em center}.mw-parser-output code.cs1-code{color:inherit;background:inherit;border:inherit;padding:inherit}.mw-parser-output .cs1-hidden-error{display:none;font-size:100%}.mw-parser-output .cs1-visible-error{font-size:100%}.mw-parser-output .cs1-maint{display:none;color:#33aa33;margin-left:0.3em}.mw-parser-output .cs1-subscription,.mw-parser-output .cs1-registration,.mw-parser-output .cs1-format{font-size:95%}.mw-parser-output .cs1-kern-left,.mw-parser-output .cs1-kern-wl-left{padding-left:0.2em}.mw-parser-output .cs1-kern-right,.mw-parser-output .cs1-kern-wl-right{padding-right:0.2em}
              </style>
             </span>
            </li>
            <li id="cite_note-2">
             <span class="mw-cite-backlink">
              <b>
               <a href="#cite_ref-2">
                ^
               </a>
              </b>
             </span>
             <span class="reference-text">
              <cite class="citation web">
               <a class="external text" href="https://web.archive.org/web/20110519093024/http://www.canadapost.ca/cpo/mc/personal/tools/mobileapp/default.jsf" rel="nofollow">
                "Mobile Apps"
               </a>
               . Canada Post. Archived from
               <a class="external text" href="http://www.canadapost.ca/cpo/mc/personal/tools/mobileapp/default.jsf" rel="nofollow">
                the original
               </a>
               on 2011-05-19.
              </cite>
              <span class="Z3988" title="ctx_ver=Z39.88-2004&amp;rft_val_fmt=info%3Aofi%2Ffmt%3Akev%3Amtx%3Abook&amp;rft.genre=unknown&amp;rft.btitle=Mobile+Apps&amp;rft.pub=Canada+Post&amp;rft_id=http%3A%2F%2Fwww.canadapost.ca%2Fcpo%2Fmc%2Fpersonal%2Ftools%2Fmobileapp%2Fdefault.jsf&amp;rfr_id=info%3Asid%2Fen.wikipedia.org%3AList+of+postal+codes+of+Canada%3A+M">
              </span>
              <link href="mw-data:TemplateStyles:r886058088" rel="mw-deduplicated-inline-style"/>
             </span>
            </li>
            <li id="cite_note-statcan-3">
             <span class="mw-cite-backlink">
              ^
              <a href="#cite_ref-statcan_3-0">
               <sup>
                <i>
                 <b>
                  a
                 </b>
                </i>
               </sup>
              </a>
              <a href="#cite_ref-statcan_3-1">
               <sup>
                <i>
                 <b>
                  b
                 </b>
                </i>
               </sup>
              </a>
             </span>
             <span class="reference-text">
              <cite class="citation web">
               <a class="external text" href="http://www12.statcan.ca/english/census06/data/popdwell/Table.cfm?T=1201&amp;SR=1&amp;S=0&amp;O=A&amp;RPP=9999&amp;PR=0&amp;CMA=0" rel="nofollow">
                "2006 Census of Population"
               </a>
               . 15 October 2008.
              </cite>
              <span class="Z3988" title="ctx_ver=Z39.88-2004&amp;rft_val_fmt=info%3Aofi%2Ffmt%3Akev%3Amtx%3Abook&amp;rft.genre=unknown&amp;rft.btitle=2006+Census+of+Population&amp;rft.date=2008-10-15&amp;rft_id=http%3A%2F%2Fwww12.statcan.ca%2Fenglish%2Fcensus06%2Fdata%2Fpopdwell%2FTable.cfm%3FT%3D1201%26SR%3D1%26S%3D0%26O%3DA%26RPP%3D9999%26PR%3D0%26CMA%3D0&amp;rfr_id=info%3Asid%2Fen.wikipedia.org%3AList+of+postal+codes+of+Canada%3A+M">
              </span>
              <link href="mw-data:TemplateStyles:r886058088" rel="mw-deduplicated-inline-style"/>
             </span>
            </li>
           </ol>
          </div>
          <table class="navbox">
           <tbody>
            <tr>
             <td style="width:36px; text-align:center">
              <a class="image" href="/wiki/File:Flag_of_Canada.svg" title="Flag of Canada">
               <img alt="Flag of Canada" data-file-height="600" data-file-width="1200" decoding="async" height="18" src="//upload.wikimedia.org/wikipedia/en/thumb/c/cf/Flag_of_Canada.svg/36px-Flag_of_Canada.svg.png" srcset="//upload.wikimedia.org/wikipedia/en/thumb/c/cf/Flag_of_Canada.svg/54px-Flag_of_Canada.svg.png 1.5x, //upload.wikimedia.org/wikipedia/en/thumb/c/cf/Flag_of_Canada.svg/72px-Flag_of_Canada.svg.png 2x" width="36"/>
              </a>
             </td>
             <th class="navbox-title" style="font-size:110%">
              <a href="/wiki/Postal_codes_in_Canada" title="Postal codes in Canada">
               Canadian postal codes
              </a>
             </th>
             <td style="width:36px; text-align:center">
              <a class="image" href="/wiki/File:Canadian_postal_district_map_(without_legends).svg">
               <img alt="Canadian postal district map (without legends).svg" data-file-height="846" data-file-width="1000" decoding="async" height="18" src="//upload.wikimedia.org/wikipedia/commons/thumb/e/eb/Canadian_postal_district_map_%28without_legends%29.svg/21px-Canadian_postal_district_map_%28without_legends%29.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/e/eb/Canadian_postal_district_map_%28without_legends%29.svg/32px-Canadian_postal_district_map_%28without_legends%29.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/e/eb/Canadian_postal_district_map_%28without_legends%29.svg/43px-Canadian_postal_district_map_%28without_legends%29.svg.png 2x" width="21"/>
              </a>
             </td>
            </tr>
            <tr>
             <td colspan="3" style="text-align:center; font-size: 100%;">
              <table cellspacing="0" style="background-color: #F8F8F8;" width="100%">
               <tbody>
                <tr>
                 <td style="text-align:center; border:1px solid #aaa;">
                  <a href="/wiki/Newfoundland_and_Labrador" title="Newfoundland and Labrador">
                   NL
                  </a>
                 </td>
                 <td style="text-align:center; border:1px solid #aaa;">
                  <a href="/wiki/Nova_Scotia" title="Nova Scotia">
                   NS
                  </a>
                 </td>
                 <td style="text-align:center; border:1px solid #aaa;">
                  <a href="/wiki/Prince_Edward_Island" title="Prince Edward Island">
                   PE
                  </a>
                 </td>
                 <td style="text-align:center; border:1px solid #aaa;">
                  <a href="/wiki/New_Brunswick" title="New Brunswick">
                   NB
                  </a>
                 </td>
                 <td colspan="3" style="text-align:center; border:1px solid #aaa;">
                  <a href="/wiki/Quebec" title="Quebec">
                   QC
                  </a>
                 </td>
                 <td colspan="5" style="text-align:center; border:1px solid #aaa;">
                  <a href="/wiki/Ontario" title="Ontario">
                   ON
                  </a>
                 </td>
                 <td style="text-align:center; border:1px solid #aaa;">
                  <a href="/wiki/Manitoba" title="Manitoba">
                   MB
                  </a>
                 </td>
                 <td style="text-align:center; border:1px solid #aaa;">
                  <a href="/wiki/Saskatchewan" title="Saskatchewan">
                   SK
                  </a>
                 </td>
                 <td style="text-align:center; border:1px solid #aaa;">
                  <a href="/wiki/Alberta" title="Alberta">
                   AB
                  </a>
                 </td>
                 <td style="text-align:center; border:1px solid #aaa;">
                  <a href="/wiki/British_Columbia" title="British Columbia">
                   BC
                  </a>
                 </td>
                 <td style="text-align:center; border:1px solid #aaa;">
                  <a href="/wiki/Nunavut" title="Nunavut">
                   NU
                  </a>
                  /
                  <a href="/wiki/Northwest_Territories" title="Northwest Territories">
                   NT
                  </a>
                 </td>
                 <td style="text-align:center; border:1px solid #aaa;">
                  <a href="/wiki/Yukon" title="Yukon">
                   YT
                  </a>
                 </td>
                </tr>
                <tr>
                 <td align="center" style="border: 1px solid #FF0000; background-color: #FFE0E0; font-size: 135%;" width="5%">
                  <a href="/wiki/List_of_postal_codes_of_Canada:_A" title="List of postal codes of Canada: A">
                   A
                  </a>
                 </td>
                 <td align="center" style="border: 1px solid #FF4000; background-color: #FFE8E0; font-size: 135%;" width="5%">
                  <a href="/wiki/List_of_postal_codes_of_Canada:_B" title="List of postal codes of Canada: B">
                   B
                  </a>
                 </td>
                 <td align="center" style="border: 1px solid #FF8000; background-color: #FFF0E0; font-size: 135%;" width="5%">
                  <a href="/wiki/List_of_postal_codes_of_Canada:_C" title="List of postal codes of Canada: C">
                   C
                  </a>
                 </td>
                 <td align="center" style="border: 1px solid #FFC000; background-color: #FFF8E0; font-size: 135%;" width="5%">
                  <a href="/wiki/List_of_postal_codes_of_Canada:_E" title="List of postal codes of Canada: E">
                   E
                  </a>
                 </td>
                 <td align="center" style="border: 1px solid #FFFF00; background-color: #FFFFE0; font-size: 135%;" width="5%">
                  <a href="/wiki/List_of_postal_codes_of_Canada:_G" title="List of postal codes of Canada: G">
                   G
                  </a>
                 </td>
                 <td align="center" style="border: 1px solid #C0FF00; background-color: #F8FFE0; font-size: 135%;" width="5%">
                  <a href="/wiki/List_of_postal_codes_of_Canada:_H" title="List of postal codes of Canada: H">
                   H
                  </a>
                 </td>
                 <td align="center" style="border: 1px solid #80FF00; background-color: #F0FFE0; font-size: 135%;" width="5%">
                  <a href="/wiki/List_of_postal_codes_of_Canada:_J" title="List of postal codes of Canada: J">
                   J
                  </a>
                 </td>
                 <td align="center" style="border: 1px solid #00FF00; background-color: #E0FFE0; font-size: 135%;" width="5%">
                  <a href="/wiki/List_of_postal_codes_of_Canada:_K" title="List of postal codes of Canada: K">
                   K
                  </a>
                 </td>
                 <td align="center" style="border: 1px solid #00FF80; background-color: #E0FFF0; font-size: 135%;" width="5%">
                  <a href="/wiki/List_of_postal_codes_of_Canada:_L" title="List of postal codes of Canada: L">
                   L
                  </a>
                 </td>
                 <td align="center" style="border: 1px solid #E0FFF8; background-color: #00FFC0; font-size: 135%; color: black;" width="5%">
                  <a class="mw-selflink selflink">
                   M
                  </a>
                 </td>
                 <td align="center" style="border: 1px solid #00FFE0; background-color: #E0FFFC; font-size: 135%;" width="5%">
                  <a href="/wiki/List_of_postal_codes_of_Canada:_N" title="List of postal codes of Canada: N">
                   N
                  </a>
                 </td>
                 <td align="center" style="border: 1px solid #00FFFF; background-color: #E0FFFF; font-size: 135%;" width="5%">
                  <a href="/wiki/List_of_postal_codes_of_Canada:_P" title="List of postal codes of Canada: P">
                   P
                  </a>
                 </td>
                 <td align="center" style="border: 1px solid #00C0FF; background-color: #E0F8FF; font-size: 135%;" width="5%">
                  <a href="/wiki/List_of_postal_codes_of_Canada:_R" title="List of postal codes of Canada: R">
                   R
                  </a>
                 </td>
                 <td align="center" style="border: 1px solid #0080FF; background-color: #E0F0FF; font-size: 135%;" width="5%">
                  <a href="/wiki/List_of_postal_codes_of_Canada:_S" title="List of postal codes of Canada: S">
                   S
                  </a>
                 </td>
                 <td align="center" style="border: 1px solid #0040FF; background-color: #E0E8FF; font-size: 135%;" width="5%">
                  <a href="/wiki/List_of_postal_codes_of_Canada:_T" title="List of postal codes of Canada: T">
                   T
                  </a>
                 </td>
                 <td align="center" style="border: 1px solid #0000FF; background-color: #E0E0FF; font-size: 135%;" width="5%">
                  <a href="/wiki/List_of_postal_codes_of_Canada:_V" title="List of postal codes of Canada: V">
                   V
                  </a>
                 </td>
                 <td align="center" style="border: 1px solid #A000FF; background-color: #E8E0FF; font-size: 135%;" width="5%">
                  <a href="/wiki/List_of_postal_codes_of_Canada:_X" title="List of postal codes of Canada: X">
                   X
                  </a>
                 </td>
                 <td align="center" style="border: 1px solid #FF00FF; background-color: #FFE0FF; font-size: 135%;" width="5%">
                  <a href="/wiki/List_of_postal_codes_of_Canada:_Y" title="List of postal codes of Canada: Y">
                   Y
                  </a>
                 </td>
                </tr>
               </tbody>
              </table>
             </td>
            </tr>
           </tbody>
          </table>
          <!-- 
    NewPP limit report
    Parsed by mw1325
    Cached time: 20190606102526
    Cache expiry: 2592000
    Dynamic content: false
    CPU time usage: 0.204 seconds
    Real time usage: 0.238 seconds
    Preprocessor visited node count: 575/1000000
    Preprocessor generated node count: 0/1500000
    Post‐expand include size: 10232/2097152 bytes
    Template argument size: 13/2097152 bytes
    Highest expansion depth: 4/40
    Expensive parser function count: 0/500
    Unstrip recursion depth: 1/20
    Unstrip post‐expand size: 9025/5000000 bytes
    Number of Wikibase entities loaded: 0/400
    Lua time usage: 0.079/10.000 seconds
    Lua memory usage: 1.71 MB/50 MB
    -->
          <!--
    Transclusion expansion time report (%,ms,calls,template)
    100.00%  137.705      1 -total
     83.72%  115.289      3 Template:Cite_web
      3.78%    5.211      1 Template:Col-2
      3.64%    5.019      1 Template:Canadian_postal_codes
      2.17%    2.986      1 Template:Col-break
      1.73%    2.385      1 Template:Col-begin
      1.33%    1.829      2 Template:Col-end
    -->
          <!-- Saved in parser cache with key enwiki:pcache:idhash:539066-0!canonical and timestamp 20190606102526 and revision id 900271985
     -->
         </div>
         <noscript>
          <img alt="" height="1" src="//en.wikipedia.org/wiki/Special:CentralAutoLogin/start?type=1x1" style="border: none; position: absolute;" title="" width="1"/>
         </noscript>
        </div>
        <div class="printfooter">
         Retrieved from "
         <a dir="ltr" href="https://en.wikipedia.org/w/index.php?title=List_of_postal_codes_of_Canada:_M&amp;oldid=900271985">
          https://en.wikipedia.org/w/index.php?title=List_of_postal_codes_of_Canada:_M&amp;oldid=900271985
         </a>
         "
        </div>
        <div class="catlinks" data-mw="interface" id="catlinks">
         <div class="mw-normal-catlinks" id="mw-normal-catlinks">
          <a href="/wiki/Help:Category" title="Help:Category">
           Categories
          </a>
          :
          <ul>
           <li>
            <a href="/wiki/Category:Communications_in_Ontario" title="Category:Communications in Ontario">
             Communications in Ontario
            </a>
           </li>
           <li>
            <a href="/wiki/Category:Postal_codes_in_Canada" title="Category:Postal codes in Canada">
             Postal codes in Canada
            </a>
           </li>
           <li>
            <a href="/wiki/Category:Toronto" title="Category:Toronto">
             Toronto
            </a>
           </li>
           <li>
            <a href="/wiki/Category:Ontario-related_lists" title="Category:Ontario-related lists">
             Ontario-related lists
            </a>
           </li>
          </ul>
         </div>
        </div>
        <div class="visualClear">
        </div>
       </div>
      </div>
      <div id="mw-navigation">
       <h2>
        Navigation menu
       </h2>
       <div id="mw-head">
        <div aria-labelledby="p-personal-label" id="p-personal" role="navigation">
         <h3 id="p-personal-label">
          Personal tools
         </h3>
         <ul>
          <li id="pt-anonuserpage">
           Not logged in
          </li>
          <li id="pt-anontalk">
           <a accesskey="n" href="/wiki/Special:MyTalk" title="Discussion about edits from this IP address [n]">
            Talk
           </a>
          </li>
          <li id="pt-anoncontribs">
           <a accesskey="y" href="/wiki/Special:MyContributions" title="A list of edits made from this IP address [y]">
            Contributions
           </a>
          </li>
          <li id="pt-createaccount">
           <a href="/w/index.php?title=Special:CreateAccount&amp;returnto=List+of+postal+codes+of+Canada%3A+M" title="You are encouraged to create an account and log in; however, it is not mandatory">
            Create account
           </a>
          </li>
          <li id="pt-login">
           <a accesskey="o" href="/w/index.php?title=Special:UserLogin&amp;returnto=List+of+postal+codes+of+Canada%3A+M" title="You're encouraged to log in; however, it's not mandatory. [o]">
            Log in
           </a>
          </li>
         </ul>
        </div>
        <div id="left-navigation">
         <div aria-labelledby="p-namespaces-label" class="vectorTabs" id="p-namespaces" role="navigation">
          <h3 id="p-namespaces-label">
           Namespaces
          </h3>
          <ul>
           <li class="selected" id="ca-nstab-main">
            <span>
             <a accesskey="c" href="/wiki/List_of_postal_codes_of_Canada:_M" title="View the content page [c]">
              Article
             </a>
            </span>
           </li>
           <li id="ca-talk">
            <span>
             <a accesskey="t" href="/wiki/Talk:List_of_postal_codes_of_Canada:_M" rel="discussion" title="Discussion about the content page [t]">
              Talk
             </a>
            </span>
           </li>
          </ul>
         </div>
         <div aria-labelledby="p-variants-label" class="vectorMenu emptyPortlet" id="p-variants" role="navigation">
          <input aria-labelledby="p-variants-label" class="vectorMenuCheckbox" type="checkbox"/>
          <h3 id="p-variants-label">
           <span>
            Variants
           </span>
          </h3>
          <ul class="menu">
          </ul>
         </div>
        </div>
        <div id="right-navigation">
         <div aria-labelledby="p-views-label" class="vectorTabs" id="p-views" role="navigation">
          <h3 id="p-views-label">
           Views
          </h3>
          <ul>
           <li class="collapsible selected" id="ca-view">
            <span>
             <a href="/wiki/List_of_postal_codes_of_Canada:_M">
              Read
             </a>
            </span>
           </li>
           <li class="collapsible" id="ca-edit">
            <span>
             <a accesskey="e" href="/w/index.php?title=List_of_postal_codes_of_Canada:_M&amp;action=edit" title="Edit this page [e]">
              Edit
             </a>
            </span>
           </li>
           <li class="collapsible" id="ca-history">
            <span>
             <a accesskey="h" href="/w/index.php?title=List_of_postal_codes_of_Canada:_M&amp;action=history" title="Past revisions of this page [h]">
              View history
             </a>
            </span>
           </li>
          </ul>
         </div>
         <div aria-labelledby="p-cactions-label" class="vectorMenu emptyPortlet" id="p-cactions" role="navigation">
          <input aria-labelledby="p-cactions-label" class="vectorMenuCheckbox" type="checkbox"/>
          <h3 id="p-cactions-label">
           <span>
            More
           </span>
          </h3>
          <ul class="menu">
          </ul>
         </div>
         <div id="p-search" role="search">
          <h3>
           <label for="searchInput">
            Search
           </label>
          </h3>
          <form action="/w/index.php" id="searchform">
           <div id="simpleSearch">
            <input accesskey="f" id="searchInput" name="search" placeholder="Search Wikipedia" title="Search Wikipedia [f]" type="search"/>
            <input name="title" type="hidden" value="Special:Search"/>
            <input class="searchButton mw-fallbackSearchButton" id="mw-searchButton" name="fulltext" title="Search Wikipedia for this text" type="submit" value="Search"/>
            <input class="searchButton" id="searchButton" name="go" title="Go to a page with this exact name if it exists" type="submit" value="Go"/>
           </div>
          </form>
         </div>
        </div>
       </div>
       <div id="mw-panel">
        <div id="p-logo" role="banner">
         <a class="mw-wiki-logo" href="/wiki/Main_Page" title="Visit the main page">
         </a>
        </div>
        <div aria-labelledby="p-navigation-label" class="portal" id="p-navigation" role="navigation">
         <h3 id="p-navigation-label">
          Navigation
         </h3>
         <div class="body">
          <ul>
           <li id="n-mainpage-description">
            <a accesskey="z" href="/wiki/Main_Page" title="Visit the main page [z]">
             Main page
            </a>
           </li>
           <li id="n-contents">
            <a href="/wiki/Portal:Contents" title="Guides to browsing Wikipedia">
             Contents
            </a>
           </li>
           <li id="n-featuredcontent">
            <a href="/wiki/Portal:Featured_content" title="Featured content – the best of Wikipedia">
             Featured content
            </a>
           </li>
           <li id="n-currentevents">
            <a href="/wiki/Portal:Current_events" title="Find background information on current events">
             Current events
            </a>
           </li>
           <li id="n-randompage">
            <a accesskey="x" href="/wiki/Special:Random" title="Load a random article [x]">
             Random article
            </a>
           </li>
           <li id="n-sitesupport">
            <a href="https://donate.wikimedia.org/wiki/Special:FundraiserRedirector?utm_source=donate&amp;utm_medium=sidebar&amp;utm_campaign=C13_en.wikipedia.org&amp;uselang=en" title="Support us">
             Donate to Wikipedia
            </a>
           </li>
           <li id="n-shoplink">
            <a href="//shop.wikimedia.org" title="Visit the Wikipedia store">
             Wikipedia store
            </a>
           </li>
          </ul>
         </div>
        </div>
        <div aria-labelledby="p-interaction-label" class="portal" id="p-interaction" role="navigation">
         <h3 id="p-interaction-label">
          Interaction
         </h3>
         <div class="body">
          <ul>
           <li id="n-help">
            <a href="/wiki/Help:Contents" title="Guidance on how to use and edit Wikipedia">
             Help
            </a>
           </li>
           <li id="n-aboutsite">
            <a href="/wiki/Wikipedia:About" title="Find out about Wikipedia">
             About Wikipedia
            </a>
           </li>
           <li id="n-portal">
            <a href="/wiki/Wikipedia:Community_portal" title="About the project, what you can do, where to find things">
             Community portal
            </a>
           </li>
           <li id="n-recentchanges">
            <a accesskey="r" href="/wiki/Special:RecentChanges" title="A list of recent changes in the wiki [r]">
             Recent changes
            </a>
           </li>
           <li id="n-contactpage">
            <a href="//en.wikipedia.org/wiki/Wikipedia:Contact_us" title="How to contact Wikipedia">
             Contact page
            </a>
           </li>
          </ul>
         </div>
        </div>
        <div aria-labelledby="p-tb-label" class="portal" id="p-tb" role="navigation">
         <h3 id="p-tb-label">
          Tools
         </h3>
         <div class="body">
          <ul>
           <li id="t-whatlinkshere">
            <a accesskey="j" href="/wiki/Special:WhatLinksHere/List_of_postal_codes_of_Canada:_M" title="List of all English Wikipedia pages containing links to this page [j]">
             What links here
            </a>
           </li>
           <li id="t-recentchangeslinked">
            <a accesskey="k" href="/wiki/Special:RecentChangesLinked/List_of_postal_codes_of_Canada:_M" rel="nofollow" title="Recent changes in pages linked from this page [k]">
             Related changes
            </a>
           </li>
           <li id="t-upload">
            <a accesskey="u" href="/wiki/Wikipedia:File_Upload_Wizard" title="Upload files [u]">
             Upload file
            </a>
           </li>
           <li id="t-specialpages">
            <a accesskey="q" href="/wiki/Special:SpecialPages" title="A list of all special pages [q]">
             Special pages
            </a>
           </li>
           <li id="t-permalink">
            <a href="/w/index.php?title=List_of_postal_codes_of_Canada:_M&amp;oldid=900271985" title="Permanent link to this revision of the page">
             Permanent link
            </a>
           </li>
           <li id="t-info">
            <a href="/w/index.php?title=List_of_postal_codes_of_Canada:_M&amp;action=info" title="More information about this page">
             Page information
            </a>
           </li>
           <li id="t-wikibase">
            <a accesskey="g" href="https://www.wikidata.org/wiki/Special:EntityPage/Q3248240" title="Link to connected data repository item [g]">
             Wikidata item
            </a>
           </li>
           <li id="t-cite">
            <a href="/w/index.php?title=Special:CiteThisPage&amp;page=List_of_postal_codes_of_Canada%3A_M&amp;id=900271985" title="Information on how to cite this page">
             Cite this page
            </a>
           </li>
          </ul>
         </div>
        </div>
        <div aria-labelledby="p-coll-print_export-label" class="portal" id="p-coll-print_export" role="navigation">
         <h3 id="p-coll-print_export-label">
          Print/export
         </h3>
         <div class="body">
          <ul>
           <li id="coll-create_a_book">
            <a href="/w/index.php?title=Special:Book&amp;bookcmd=book_creator&amp;referer=List+of+postal+codes+of+Canada%3A+M">
             Create a book
            </a>
           </li>
           <li id="coll-download-as-rl">
            <a href="/w/index.php?title=Special:ElectronPdf&amp;page=List+of+postal+codes+of+Canada%3A+M&amp;action=show-download-screen">
             Download as PDF
            </a>
           </li>
           <li id="t-print">
            <a accesskey="p" href="/w/index.php?title=List_of_postal_codes_of_Canada:_M&amp;printable=yes" title="Printable version of this page [p]">
             Printable version
            </a>
           </li>
          </ul>
         </div>
        </div>
        <div aria-labelledby="p-lang-label" class="portal" id="p-lang" role="navigation">
         <h3 id="p-lang-label">
          Languages
         </h3>
         <div class="body">
          <ul>
           <li class="interlanguage-link interwiki-fr">
            <a class="interlanguage-link-target" href="https://fr.wikipedia.org/wiki/Liste_des_codes_postaux_canadiens_d%C3%A9butant_par_M" hreflang="fr" lang="fr" title="Liste des codes postaux canadiens débutant par M – French">
             Français
            </a>
           </li>
          </ul>
          <div class="after-portlet after-portlet-lang">
           <span class="wb-langlinks-edit wb-langlinks-link">
            <a class="wbc-editpage" href="https://www.wikidata.org/wiki/Special:EntityPage/Q3248240#sitelinks-wikipedia" title="Edit interlanguage links">
             Edit links
            </a>
           </span>
          </div>
         </div>
        </div>
       </div>
      </div>
      <div id="footer" role="contentinfo">
       <ul id="footer-info">
        <li id="footer-info-lastmod">
         This page was last edited on 4 June 2019, at 14:58
         <span class="anonymous-show">
          (UTC)
         </span>
         .
        </li>
        <li id="footer-info-copyright">
         Text is available under the
         <a href="//en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License" rel="license">
          Creative Commons Attribution-ShareAlike License
         </a>
         <a href="//creativecommons.org/licenses/by-sa/3.0/" rel="license" style="display:none;">
         </a>
         ;
    additional terms may apply.  By using this site, you agree to the
         <a href="//foundation.wikimedia.org/wiki/Terms_of_Use">
          Terms of Use
         </a>
         and
         <a href="//foundation.wikimedia.org/wiki/Privacy_policy">
          Privacy Policy
         </a>
         . Wikipedia® is a registered trademark of the
         <a href="//www.wikimediafoundation.org/">
          Wikimedia Foundation, Inc.
         </a>
         , a non-profit organization.
        </li>
       </ul>
       <ul id="footer-places">
        <li id="footer-places-privacy">
         <a class="extiw" href="https://foundation.wikimedia.org/wiki/Privacy_policy" title="wmf:Privacy policy">
          Privacy policy
         </a>
        </li>
        <li id="footer-places-about">
         <a href="/wiki/Wikipedia:About" title="Wikipedia:About">
          About Wikipedia
         </a>
        </li>
        <li id="footer-places-disclaimer">
         <a href="/wiki/Wikipedia:General_disclaimer" title="Wikipedia:General disclaimer">
          Disclaimers
         </a>
        </li>
        <li id="footer-places-contact">
         <a href="//en.wikipedia.org/wiki/Wikipedia:Contact_us">
          Contact Wikipedia
         </a>
        </li>
        <li id="footer-places-developers">
         <a href="https://www.mediawiki.org/wiki/Special:MyLanguage/How_to_contribute">
          Developers
         </a>
        </li>
        <li id="footer-places-cookiestatement">
         <a href="https://foundation.wikimedia.org/wiki/Cookie_statement">
          Cookie statement
         </a>
        </li>
        <li id="footer-places-mobileview">
         <a class="noprint stopMobileRedirectToggle" href="//en.m.wikipedia.org/w/index.php?title=List_of_postal_codes_of_Canada:_M&amp;mobileaction=toggle_view_mobile">
          Mobile view
         </a>
        </li>
       </ul>
       <ul class="noprint" id="footer-icons">
        <li id="footer-copyrightico">
         <a href="https://wikimediafoundation.org/">
          <img alt="Wikimedia Foundation" height="31" src="/static/images/wikimedia-button.png" srcset="/static/images/wikimedia-button-1.5x.png 1.5x, /static/images/wikimedia-button-2x.png 2x" width="88"/>
         </a>
        </li>
        <li id="footer-poweredbyico">
         <a href="https://www.mediawiki.org/">
          <img alt="Powered by MediaWiki" height="31" src="/static/images/poweredby_mediawiki_88x31.png" srcset="/static/images/poweredby_mediawiki_132x47.png 1.5x, /static/images/poweredby_mediawiki_176x62.png 2x" width="88"/>
         </a>
        </li>
       </ul>
       <div style="clear: both;">
       </div>
      </div>
      <script>
       (RLQ=window.RLQ||[]).push(function(){mw.config.set({"wgPageParseReport":{"limitreport":{"cputime":"0.204","walltime":"0.238","ppvisitednodes":{"value":575,"limit":1000000},"ppgeneratednodes":{"value":0,"limit":1500000},"postexpandincludesize":{"value":10232,"limit":2097152},"templateargumentsize":{"value":13,"limit":2097152},"expansiondepth":{"value":4,"limit":40},"expensivefunctioncount":{"value":0,"limit":500},"unstrip-depth":{"value":1,"limit":20},"unstrip-size":{"value":9025,"limit":5000000},"entityaccesscount":{"value":0,"limit":400},"timingprofile":["100.00%  137.705      1 -total"," 83.72%  115.289      3 Template:Cite_web","  3.78%    5.211      1 Template:Col-2","  3.64%    5.019      1 Template:Canadian_postal_codes","  2.17%    2.986      1 Template:Col-break","  1.73%    2.385      1 Template:Col-begin","  1.33%    1.829      2 Template:Col-end"]},"scribunto":{"limitreport-timeusage":{"value":"0.079","limit":"10.000"},"limitreport-memusage":{"value":1788769,"limit":52428800}},"cachereport":{"origin":"mw1325","timestamp":"20190606102526","ttl":2592000,"transientcontent":false}}});});
      </script>
      <script type="application/ld+json">
       {"@context":"https:\/\/schema.org","@type":"Article","name":"List of postal codes of Canada: M","url":"https:\/\/en.wikipedia.org\/wiki\/List_of_postal_codes_of_Canada:_M","sameAs":"http:\/\/www.wikidata.org\/entity\/Q3248240","mainEntity":"http:\/\/www.wikidata.org\/entity\/Q3248240","author":{"@type":"Organization","name":"Contributors to Wikimedia projects"},"publisher":{"@type":"Organization","name":"Wikimedia Foundation, Inc.","logo":{"@type":"ImageObject","url":"https:\/\/www.wikimedia.org\/static\/images\/wmf-hor-googpub.png"}},"datePublished":"2004-03-20T10:02:13Z","dateModified":"2019-06-04T14:58:52Z","headline":"Wikimedia list article"}
      </script>
      <script>
       (RLQ=window.RLQ||[]).push(function(){mw.config.set({"wgBackendResponseTime":351,"wgHostname":"mw1325"});});
      </script>
     </body>
    </html>
    


__Table to dataframe__


```python
table = soup.find('tbody')
rows = table.select('tr')
row = [r.get_text() for r in rows]
```


```python
df = pd.DataFrame(row)
df1 = df[0].str.split('\n', expand=True)
df2 = df1.rename(columns=df1.iloc[0])
df3 = df2.drop(df2.index[0])
df4 = df3[df3.Borough != 'Not assigned']
df5 = df4.groupby(['Postcode', 'Borough'], sort = False).agg(','.join)
df5.reset_index(inplace = True)
df6 = df5.replace("Not assigned", "Queen's Park")
df6.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Postcode</th>
      <th>Borough</th>
      <th>Neighbourhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Harbourfront,Regent Park</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Heights,Lawrence Manor</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M7A</td>
      <td>Queen's Park</td>
      <td>Queen's Park</td>
    </tr>
  </tbody>
</table>
</div>



__Last cell in part 1 print number of rows in df__


```python
df6.shape
```




    (103, 3)



__Part 2 geocoding (used given CSV file)__


```python
url = "http://cocl.us/Geospatial_data"
df7 = pd.read_csv(url)
df7.rename(columns={'Postal Code': 'Postcode'}, inplace=True) 
df7.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Postcode</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M1B</td>
      <td>43.806686</td>
      <td>-79.194353</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M1C</td>
      <td>43.784535</td>
      <td>-79.160497</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M1E</td>
      <td>43.763573</td>
      <td>-79.188711</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M1G</td>
      <td>43.770992</td>
      <td>-79.216917</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M1H</td>
      <td>43.773136</td>
      <td>-79.239476</td>
    </tr>
  </tbody>
</table>
</div>




```python
df8 = pd.merge(df6, df7, on='Postcode')
df8.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Postcode</th>
      <th>Borough</th>
      <th>Neighbourhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
      <td>43.753259</td>
      <td>-79.329656</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
      <td>43.725882</td>
      <td>-79.315572</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Harbourfront,Regent Park</td>
      <td>43.654260</td>
      <td>-79.360636</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Heights,Lawrence Manor</td>
      <td>43.718518</td>
      <td>-79.464763</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M7A</td>
      <td>Queen's Park</td>
      <td>Queen's Park</td>
      <td>43.662301</td>
      <td>-79.389494</td>
    </tr>
  </tbody>
</table>
</div>



__Part 3 Took only boroughs that contain the word 'Toronto'__


```python
to_df=df8[df8['Borough'].str.contains('Toronto')]
to_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Postcode</th>
      <th>Borough</th>
      <th>Neighbourhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Harbourfront,Regent Park</td>
      <td>43.654260</td>
      <td>-79.360636</td>
    </tr>
    <tr>
      <th>9</th>
      <td>M5B</td>
      <td>Downtown Toronto</td>
      <td>Ryerson,Garden District</td>
      <td>43.657162</td>
      <td>-79.378937</td>
    </tr>
    <tr>
      <th>15</th>
      <td>M5C</td>
      <td>Downtown Toronto</td>
      <td>St. James Town</td>
      <td>43.651494</td>
      <td>-79.375418</td>
    </tr>
    <tr>
      <th>19</th>
      <td>M4E</td>
      <td>East Toronto</td>
      <td>The Beaches</td>
      <td>43.676357</td>
      <td>-79.293031</td>
    </tr>
    <tr>
      <th>20</th>
      <td>M5E</td>
      <td>Downtown Toronto</td>
      <td>Berczy Park</td>
      <td>43.644771</td>
      <td>-79.373306</td>
    </tr>
    <tr>
      <th>24</th>
      <td>M5G</td>
      <td>Downtown Toronto</td>
      <td>Central Bay Street</td>
      <td>43.657952</td>
      <td>-79.387383</td>
    </tr>
    <tr>
      <th>25</th>
      <td>M6G</td>
      <td>Downtown Toronto</td>
      <td>Christie</td>
      <td>43.669542</td>
      <td>-79.422564</td>
    </tr>
    <tr>
      <th>30</th>
      <td>M5H</td>
      <td>Downtown Toronto</td>
      <td>Adelaide,King,Richmond</td>
      <td>43.650571</td>
      <td>-79.384568</td>
    </tr>
    <tr>
      <th>31</th>
      <td>M6H</td>
      <td>West Toronto</td>
      <td>Dovercourt Village,Dufferin</td>
      <td>43.669005</td>
      <td>-79.442259</td>
    </tr>
    <tr>
      <th>36</th>
      <td>M5J</td>
      <td>Downtown Toronto</td>
      <td>Harbourfront East,Toronto Islands,Union Station</td>
      <td>43.640816</td>
      <td>-79.381752</td>
    </tr>
    <tr>
      <th>37</th>
      <td>M6J</td>
      <td>West Toronto</td>
      <td>Little Portugal,Trinity</td>
      <td>43.647927</td>
      <td>-79.419750</td>
    </tr>
    <tr>
      <th>41</th>
      <td>M4K</td>
      <td>East Toronto</td>
      <td>The Danforth West,Riverdale</td>
      <td>43.679557</td>
      <td>-79.352188</td>
    </tr>
    <tr>
      <th>42</th>
      <td>M5K</td>
      <td>Downtown Toronto</td>
      <td>Design Exchange,Toronto Dominion Centre</td>
      <td>43.647177</td>
      <td>-79.381576</td>
    </tr>
    <tr>
      <th>43</th>
      <td>M6K</td>
      <td>West Toronto</td>
      <td>Brockton,Exhibition Place,Parkdale Village</td>
      <td>43.636847</td>
      <td>-79.428191</td>
    </tr>
    <tr>
      <th>47</th>
      <td>M4L</td>
      <td>East Toronto</td>
      <td>The Beaches West,India Bazaar</td>
      <td>43.668999</td>
      <td>-79.315572</td>
    </tr>
    <tr>
      <th>48</th>
      <td>M5L</td>
      <td>Downtown Toronto</td>
      <td>Commerce Court,Victoria Hotel</td>
      <td>43.648198</td>
      <td>-79.379817</td>
    </tr>
    <tr>
      <th>54</th>
      <td>M4M</td>
      <td>East Toronto</td>
      <td>Studio District</td>
      <td>43.659526</td>
      <td>-79.340923</td>
    </tr>
    <tr>
      <th>61</th>
      <td>M4N</td>
      <td>Central Toronto</td>
      <td>Lawrence Park</td>
      <td>43.728020</td>
      <td>-79.388790</td>
    </tr>
    <tr>
      <th>62</th>
      <td>M5N</td>
      <td>Central Toronto</td>
      <td>Roselawn</td>
      <td>43.711695</td>
      <td>-79.416936</td>
    </tr>
    <tr>
      <th>67</th>
      <td>M4P</td>
      <td>Central Toronto</td>
      <td>Davisville North</td>
      <td>43.712751</td>
      <td>-79.390197</td>
    </tr>
    <tr>
      <th>68</th>
      <td>M5P</td>
      <td>Central Toronto</td>
      <td>Forest Hill North,Forest Hill West</td>
      <td>43.696948</td>
      <td>-79.411307</td>
    </tr>
    <tr>
      <th>69</th>
      <td>M6P</td>
      <td>West Toronto</td>
      <td>High Park,The Junction South</td>
      <td>43.661608</td>
      <td>-79.464763</td>
    </tr>
    <tr>
      <th>73</th>
      <td>M4R</td>
      <td>Central Toronto</td>
      <td>North Toronto West</td>
      <td>43.715383</td>
      <td>-79.405678</td>
    </tr>
    <tr>
      <th>74</th>
      <td>M5R</td>
      <td>Central Toronto</td>
      <td>The Annex,North Midtown,Yorkville</td>
      <td>43.672710</td>
      <td>-79.405678</td>
    </tr>
    <tr>
      <th>75</th>
      <td>M6R</td>
      <td>West Toronto</td>
      <td>Parkdale,Roncesvalles</td>
      <td>43.648960</td>
      <td>-79.456325</td>
    </tr>
    <tr>
      <th>79</th>
      <td>M4S</td>
      <td>Central Toronto</td>
      <td>Davisville</td>
      <td>43.704324</td>
      <td>-79.388790</td>
    </tr>
    <tr>
      <th>80</th>
      <td>M5S</td>
      <td>Downtown Toronto</td>
      <td>Harbord,University of Toronto</td>
      <td>43.662696</td>
      <td>-79.400049</td>
    </tr>
    <tr>
      <th>81</th>
      <td>M6S</td>
      <td>West Toronto</td>
      <td>Runnymede,Swansea</td>
      <td>43.651571</td>
      <td>-79.484450</td>
    </tr>
    <tr>
      <th>83</th>
      <td>M4T</td>
      <td>Central Toronto</td>
      <td>Moore Park,Summerhill East</td>
      <td>43.689574</td>
      <td>-79.383160</td>
    </tr>
    <tr>
      <th>84</th>
      <td>M5T</td>
      <td>Downtown Toronto</td>
      <td>Chinatown,Grange Park,Kensington Market</td>
      <td>43.653206</td>
      <td>-79.400049</td>
    </tr>
    <tr>
      <th>86</th>
      <td>M4V</td>
      <td>Central Toronto</td>
      <td>Deer Park,Forest Hill SE,Rathnelly,South Hill,...</td>
      <td>43.686412</td>
      <td>-79.400049</td>
    </tr>
    <tr>
      <th>87</th>
      <td>M5V</td>
      <td>Downtown Toronto</td>
      <td>CN Tower,Bathurst Quay,Island airport,Harbourf...</td>
      <td>43.628947</td>
      <td>-79.394420</td>
    </tr>
    <tr>
      <th>91</th>
      <td>M4W</td>
      <td>Downtown Toronto</td>
      <td>Rosedale</td>
      <td>43.679563</td>
      <td>-79.377529</td>
    </tr>
    <tr>
      <th>92</th>
      <td>M5W</td>
      <td>Downtown Toronto</td>
      <td>Stn A PO Boxes 25 The Esplanade</td>
      <td>43.646435</td>
      <td>-79.374846</td>
    </tr>
    <tr>
      <th>96</th>
      <td>M4X</td>
      <td>Downtown Toronto</td>
      <td>Cabbagetown,St. James Town</td>
      <td>43.667967</td>
      <td>-79.367675</td>
    </tr>
    <tr>
      <th>97</th>
      <td>M5X</td>
      <td>Downtown Toronto</td>
      <td>First Canadian Place,Underground city</td>
      <td>43.648429</td>
      <td>-79.382280</td>
    </tr>
    <tr>
      <th>99</th>
      <td>M4Y</td>
      <td>Downtown Toronto</td>
      <td>Church and Wellesley</td>
      <td>43.665860</td>
      <td>-79.383160</td>
    </tr>
    <tr>
      <th>100</th>
      <td>M7Y</td>
      <td>East Toronto</td>
      <td>Business Reply Mail Processing Centre 969 Eastern</td>
      <td>43.662744</td>
      <td>-79.321558</td>
    </tr>
  </tbody>
</table>
</div>



__Beautiful Visualization__


```python
address = 'Toronto'
geolocator = Nominatim(user_agent="to_explorer")
location = geolocator.geocode(address)
latitude = location.latitude
longitude = location.longitude

Toronto_map = folium.Map(location=[latitude, longitude], zoom_start=12)

for lat, lng, borough, neighborhood in zip(to_df['Latitude'], to_df['Longitude'], 
                                           to_df['Borough'], to_df['Neighbourhood']):
    label = '{}, {}'.format(neighborhood, borough)
    label = folium.Popup(label, parse_html=True)
    folium.CircleMarker(
        [lat, lng],
        radius=4,
        popup=label,
        color='red',
        fill=False,
        parse_html=False).add_to(Toronto_map)  
    
Toronto_map
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><iframe src="data:text/html;charset=utf-8;base64,PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgPHNjcmlwdD5MX1BSRUZFUl9DQU5WQVMgPSBmYWxzZTsgTF9OT19UT1VDSCA9IGZhbHNlOyBMX0RJU0FCTEVfM0QgPSBmYWxzZTs8L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2FqYXguZ29vZ2xlYXBpcy5jb20vYWpheC9saWJzL2pxdWVyeS8xLjExLjEvanF1ZXJ5Lm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvanMvYm9vdHN0cmFwLm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvTGVhZmxldC5hd2Vzb21lLW1hcmtlcnMvMi4wLjIvbGVhZmxldC5hd2Vzb21lLW1hcmtlcnMuanMiPjwvc2NyaXB0PgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvY3NzL2Jvb3RzdHJhcC10aGVtZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vZm9udC1hd2Vzb21lLzQuNi4zL2Nzcy9mb250LWF3ZXNvbWUubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9yYXdnaXQuY29tL3B5dGhvbi12aXN1YWxpemF0aW9uL2ZvbGl1bS9tYXN0ZXIvZm9saXVtL3RlbXBsYXRlcy9sZWFmbGV0LmF3ZXNvbWUucm90YXRlLmNzcyIvPgogICAgPHN0eWxlPmh0bWwsIGJvZHkge3dpZHRoOiAxMDAlO2hlaWdodDogMTAwJTttYXJnaW46IDA7cGFkZGluZzogMDt9PC9zdHlsZT4KICAgIDxzdHlsZT4jbWFwIHtwb3NpdGlvbjphYnNvbHV0ZTt0b3A6MDtib3R0b206MDtyaWdodDowO2xlZnQ6MDt9PC9zdHlsZT4KICAgIAogICAgICAgICAgICA8c3R5bGU+ICNtYXBfNTdiOWNhZDZhNzk2NDMxZmIwOWJhNTM1Y2NmZTg5ZDAgewogICAgICAgICAgICAgICAgcG9zaXRpb24gOiByZWxhdGl2ZTsKICAgICAgICAgICAgICAgIHdpZHRoIDogMTAwLjAlOwogICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICBsZWZ0OiAwLjAlOwogICAgICAgICAgICAgICAgdG9wOiAwLjAlOwogICAgICAgICAgICAgICAgfQogICAgICAgICAgICA8L3N0eWxlPgogICAgICAgIAo8L2hlYWQ+Cjxib2R5PiAgICAKICAgIAogICAgICAgICAgICA8ZGl2IGNsYXNzPSJmb2xpdW0tbWFwIiBpZD0ibWFwXzU3YjljYWQ2YTc5NjQzMWZiMDliYTUzNWNjZmU4OWQwIiA+PC9kaXY+CiAgICAgICAgCjwvYm9keT4KPHNjcmlwdD4gICAgCiAgICAKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGJvdW5kcyA9IG51bGw7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgdmFyIG1hcF81N2I5Y2FkNmE3OTY0MzFmYjA5YmE1MzVjY2ZlODlkMCA9IEwubWFwKAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgJ21hcF81N2I5Y2FkNmE3OTY0MzFmYjA5YmE1MzVjY2ZlODlkMCcsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB7Y2VudGVyOiBbNDMuNjUzOTYzLC03OS4zODcyMDddLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgem9vbTogMTIsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBtYXhCb3VuZHM6IGJvdW5kcywKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIGxheWVyczogW10sCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB3b3JsZENvcHlKdW1wOiBmYWxzZSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIGNyczogTC5DUlMuRVBTRzM4NTcKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgfSk7CiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciB0aWxlX2xheWVyXzY4Y2JmNTgwOWU4ZjRlOGNhNTNiYzk1MGYyZDBiOTU5ID0gTC50aWxlTGF5ZXIoCiAgICAgICAgICAgICAgICAnaHR0cHM6Ly97c30udGlsZS5vcGVuc3RyZWV0bWFwLm9yZy97en0ve3h9L3t5fS5wbmcnLAogICAgICAgICAgICAgICAgewogICJhdHRyaWJ1dGlvbiI6IG51bGwsCiAgImRldGVjdFJldGluYSI6IGZhbHNlLAogICJtYXhab29tIjogMTgsCiAgIm1pblpvb20iOiAxLAogICJub1dyYXAiOiBmYWxzZSwKICAic3ViZG9tYWlucyI6ICJhYmMiCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzU3YjljYWQ2YTc5NjQzMWZiMDliYTUzNWNjZmU4OWQwKTsKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl83ZWM3MjM2ODA0Mzk0MGI2OWZmYTM2NzZiZGI2NjI0NCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1NDI1OTksLTc5LjM2MDYzNTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogInJlZCIsCiAgImZpbGxPcGFjaXR5IjogMC4yLAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF81N2I5Y2FkNmE3OTY0MzFmYjA5YmE1MzVjY2ZlODlkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jYmUwMTE0ZmEzMTc0ZDIzYTJkYjk5NTA1MjhjYzUwMCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8xODMwMTQxZTZlOTM0MTdjOGE3NzhiOWQ1ZGE2MTdhNyA9ICQoJzxkaXYgaWQ9Imh0bWxfMTgzMDE0MWU2ZTkzNDE3YzhhNzc4YjlkNWRhNjE3YTciIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkhhcmJvdXJmcm9udCxSZWdlbnQgUGFyaywgRG93bnRvd24gVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfY2JlMDExNGZhMzE3NGQyM2EyZGI5OTUwNTI4Y2M1MDAuc2V0Q29udGVudChodG1sXzE4MzAxNDFlNmU5MzQxN2M4YTc3OGI5ZDVkYTYxN2E3KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzdlYzcyMzY4MDQzOTQwYjY5ZmZhMzY3NmJkYjY2MjQ0LmJpbmRQb3B1cChwb3B1cF9jYmUwMTE0ZmEzMTc0ZDIzYTJkYjk5NTA1MjhjYzUwMCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84NGM2ZTY4MzUwOWQ0MGFjOTJiNWVlMGQ1OTU4MWIzNiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1NzE2MTgsLTc5LjM3ODkzNzA5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICJyZWQiLAogICJmaWxsT3BhY2l0eSI6IDAuMiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDQsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNTdiOWNhZDZhNzk2NDMxZmIwOWJhNTM1Y2NmZTg5ZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNDc3ZDNkYWE5MDI2NDdjZDkyMzg4MmUyYjcwOTg0ZDcgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfN2ZmMjUzOWEwMTUyNGM0MmE5NjE3YjYyYTNkNTAwNTQgPSAkKCc8ZGl2IGlkPSJodG1sXzdmZjI1MzlhMDE1MjRjNDJhOTYxN2I2MmEzZDUwMDU0IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5SeWVyc29uLEdhcmRlbiBEaXN0cmljdCwgRG93bnRvd24gVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNDc3ZDNkYWE5MDI2NDdjZDkyMzg4MmUyYjcwOTg0ZDcuc2V0Q29udGVudChodG1sXzdmZjI1MzlhMDE1MjRjNDJhOTYxN2I2MmEzZDUwMDU0KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzg0YzZlNjgzNTA5ZDQwYWM5MmI1ZWUwZDU5NTgxYjM2LmJpbmRQb3B1cChwb3B1cF80NzdkM2RhYTkwMjY0N2NkOTIzODgyZTJiNzA5ODRkNyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9hYzk1ODU4ZDE4Mzg0OTNhOWRiMzY5NWI4YmQ1OTY4MiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1MTQ5MzksLTc5LjM3NTQxNzldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogInJlZCIsCiAgImZpbGxPcGFjaXR5IjogMC4yLAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF81N2I5Y2FkNmE3OTY0MzFmYjA5YmE1MzVjY2ZlODlkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9lZDA4ODliM2ZiYmE0MmNhOGQ3MmEwNDQzODMxOTk0OCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF84NjhhZjNlNmY3OWU0YjI4ODkwNjA5ODliY2E0YjYxOSA9ICQoJzxkaXYgaWQ9Imh0bWxfODY4YWYzZTZmNzllNGIyODg5MDYwOTg5YmNhNGI2MTkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlN0LiBKYW1lcyBUb3duLCBEb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9lZDA4ODliM2ZiYmE0MmNhOGQ3MmEwNDQzODMxOTk0OC5zZXRDb250ZW50KGh0bWxfODY4YWYzZTZmNzllNGIyODg5MDYwOTg5YmNhNGI2MTkpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYWM5NTg1OGQxODM4NDkzYTlkYjM2OTViOGJkNTk2ODIuYmluZFBvcHVwKHBvcHVwX2VkMDg4OWIzZmJiYTQyY2E4ZDcyYTA0NDM4MzE5OTQ4KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzk2MDgzNGEzM2E1MjRiMmQ4ZDgyN2ZlODQ0YTdhMzU5ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjc2MzU3Mzk5OTk5OTksLTc5LjI5MzAzMTJdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogInJlZCIsCiAgImZpbGxPcGFjaXR5IjogMC4yLAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF81N2I5Y2FkNmE3OTY0MzFmYjA5YmE1MzVjY2ZlODlkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xMTVlNDIxYWY2ZDk0ZWZjYjU3YjhhYzFkY2IyZGE4ZCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8zZGVmOWU0ZTUzNzI0ZmQ2YTRkNDQ4ZWViY2M0MDFkMCA9ICQoJzxkaXYgaWQ9Imh0bWxfM2RlZjllNGU1MzcyNGZkNmE0ZDQ0OGVlYmNjNDAxZDAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlRoZSBCZWFjaGVzLCBFYXN0IFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzExNWU0MjFhZjZkOTRlZmNiNTdiOGFjMWRjYjJkYThkLnNldENvbnRlbnQoaHRtbF8zZGVmOWU0ZTUzNzI0ZmQ2YTRkNDQ4ZWViY2M0MDFkMCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85NjA4MzRhMzNhNTI0YjJkOGQ4MjdmZTg0NGE3YTM1OS5iaW5kUG9wdXAocG9wdXBfMTE1ZTQyMWFmNmQ5NGVmY2I1N2I4YWMxZGNiMmRhOGQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYmZiMGFiNGI2YjA3NDg4NTk2MTQ2MjVlOWE4ODBkODYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDQ3NzA3OTk5OTk5OTYsLTc5LjM3MzMwNjRdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogInJlZCIsCiAgImZpbGxPcGFjaXR5IjogMC4yLAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF81N2I5Y2FkNmE3OTY0MzFmYjA5YmE1MzVjY2ZlODlkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9mMjBjMTM4Nzg5MTc0ZDZhOWYxMmZkZGYwNGNhMzk1YSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF84NzBkYmE0N2QxMTk0ODFlOGJjNTNjZThhODVkYzYwNCA9ICQoJzxkaXYgaWQ9Imh0bWxfODcwZGJhNDdkMTE5NDgxZThiYzUzY2U4YTg1ZGM2MDQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkJlcmN6eSBQYXJrLCBEb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9mMjBjMTM4Nzg5MTc0ZDZhOWYxMmZkZGYwNGNhMzk1YS5zZXRDb250ZW50KGh0bWxfODcwZGJhNDdkMTE5NDgxZThiYzUzY2U4YTg1ZGM2MDQpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYmZiMGFiNGI2YjA3NDg4NTk2MTQ2MjVlOWE4ODBkODYuYmluZFBvcHVwKHBvcHVwX2YyMGMxMzg3ODkxNzRkNmE5ZjEyZmRkZjA0Y2EzOTVhKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2JiZmM5YzM4ZjUxNzQ2ZWQ5ZTY5ZDNiZjI4NThmNjY5ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjU3OTUyNCwtNzkuMzg3MzgyNl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAicmVkIiwKICAiZmlsbE9wYWNpdHkiOiAwLjIsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA0LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzU3YjljYWQ2YTc5NjQzMWZiMDliYTUzNWNjZmU4OWQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2MxNDJkMTk4MTQyZjRiOWQ4ZmZmNzlmN2IwNzExMDg5ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2EyYWFhNGQ1NjlkZDQyZDJiZmUxYzE3OTA0NzBlYTA1ID0gJCgnPGRpdiBpZD0iaHRtbF9hMmFhYTRkNTY5ZGQ0MmQyYmZlMWMxNzkwNDcwZWEwNSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Q2VudHJhbCBCYXkgU3RyZWV0LCBEb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9jMTQyZDE5ODE0MmY0YjlkOGZmZjc5ZjdiMDcxMTA4OS5zZXRDb250ZW50KGh0bWxfYTJhYWE0ZDU2OWRkNDJkMmJmZTFjMTc5MDQ3MGVhMDUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYmJmYzljMzhmNTE3NDZlZDllNjlkM2JmMjg1OGY2NjkuYmluZFBvcHVwKHBvcHVwX2MxNDJkMTk4MTQyZjRiOWQ4ZmZmNzlmN2IwNzExMDg5KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2I5MTUwZmMzNzA1MjQzYmRiYmRlNWU1NDc4ODQ0ZGYyID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjY5NTQyLC03OS40MjI1NjM3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICJyZWQiLAogICJmaWxsT3BhY2l0eSI6IDAuMiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDQsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNTdiOWNhZDZhNzk2NDMxZmIwOWJhNTM1Y2NmZTg5ZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMmY3M2RjNDZmNWVhNGI4NGIyOGRhZTMzMmU0MTc3NjAgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfN2Q4OTA4YzEzN2ZmNDRiM2EzY2VlZDdjOGVlZGNmMjYgPSAkKCc8ZGl2IGlkPSJodG1sXzdkODkwOGMxMzdmZjQ0YjNhM2NlZWQ3YzhlZWRjZjI2IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5DaHJpc3RpZSwgRG93bnRvd24gVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMmY3M2RjNDZmNWVhNGI4NGIyOGRhZTMzMmU0MTc3NjAuc2V0Q29udGVudChodG1sXzdkODkwOGMxMzdmZjQ0YjNhM2NlZWQ3YzhlZWRjZjI2KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2I5MTUwZmMzNzA1MjQzYmRiYmRlNWU1NDc4ODQ0ZGYyLmJpbmRQb3B1cChwb3B1cF8yZjczZGM0NmY1ZWE0Yjg0YjI4ZGFlMzMyZTQxNzc2MCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9lZWNhMjRkMDM2NTc0Nzg1YjY4NDdiOGJlOWQ2YzdiMCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1MDU3MTIwMDAwMDAxLC03OS4zODQ1Njc1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICJyZWQiLAogICJmaWxsT3BhY2l0eSI6IDAuMiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDQsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNTdiOWNhZDZhNzk2NDMxZmIwOWJhNTM1Y2NmZTg5ZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfODAwOTQ4OTJmY2UyNGE5OTkyYjNiNWUyMTY2ZjhmYTMgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYzQwMzQzZDljNDBmNGQzYTgzYmExNWRhNGQzYjhmN2MgPSAkKCc8ZGl2IGlkPSJodG1sX2M0MDM0M2Q5YzQwZjRkM2E4M2JhMTVkYTRkM2I4ZjdjIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5BZGVsYWlkZSxLaW5nLFJpY2htb25kLCBEb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF84MDA5NDg5MmZjZTI0YTk5OTJiM2I1ZTIxNjZmOGZhMy5zZXRDb250ZW50KGh0bWxfYzQwMzQzZDljNDBmNGQzYTgzYmExNWRhNGQzYjhmN2MpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZWVjYTI0ZDAzNjU3NDc4NWI2ODQ3YjhiZTlkNmM3YjAuYmluZFBvcHVwKHBvcHVwXzgwMDk0ODkyZmNlMjRhOTk5MmIzYjVlMjE2NmY4ZmEzKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzQxNTQ2NzY1NWVkMzQ3NDU5ZjM5MzAyMzVhODBjNWZmID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjY5MDA1MTAwMDAwMDEsLTc5LjQ0MjI1OTNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogInJlZCIsCiAgImZpbGxPcGFjaXR5IjogMC4yLAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF81N2I5Y2FkNmE3OTY0MzFmYjA5YmE1MzVjY2ZlODlkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jYTA4ZDg5Yzc1Yjk0MDYxYTdhMWZjZDQ2ZTY3MGRhOCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF80M2VhM2JlOTIyODc0MDBlYTRmMGZjNjYxY2Y3MTU2ZSA9ICQoJzxkaXYgaWQ9Imh0bWxfNDNlYTNiZTkyMjg3NDAwZWE0ZjBmYzY2MWNmNzE1NmUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRvdmVyY291cnQgVmlsbGFnZSxEdWZmZXJpbiwgV2VzdCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9jYTA4ZDg5Yzc1Yjk0MDYxYTdhMWZjZDQ2ZTY3MGRhOC5zZXRDb250ZW50KGh0bWxfNDNlYTNiZTkyMjg3NDAwZWE0ZjBmYzY2MWNmNzE1NmUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNDE1NDY3NjU1ZWQzNDc0NTlmMzkzMDIzNWE4MGM1ZmYuYmluZFBvcHVwKHBvcHVwX2NhMDhkODljNzViOTQwNjFhN2ExZmNkNDZlNjcwZGE4KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzUwZWE1ZGYxOTk5MzRiNTZiYTcxNDkwNmY5YTY2M2IwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjQwODE1NywtNzkuMzgxNzUyMjk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogInJlZCIsCiAgImZpbGxPcGFjaXR5IjogMC4yLAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF81N2I5Y2FkNmE3OTY0MzFmYjA5YmE1MzVjY2ZlODlkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9lYjI5YmU3NmU3OTc0MjM2OWU4MDlmMjQ2ZmU0YjI4NyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jNmM3Mzg0ODNlMzc0NDg1YmJiNTc2MjJmNzM5MmM4YiA9ICQoJzxkaXYgaWQ9Imh0bWxfYzZjNzM4NDgzZTM3NDQ4NWJiYjU3NjIyZjczOTJjOGIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkhhcmJvdXJmcm9udCBFYXN0LFRvcm9udG8gSXNsYW5kcyxVbmlvbiBTdGF0aW9uLCBEb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9lYjI5YmU3NmU3OTc0MjM2OWU4MDlmMjQ2ZmU0YjI4Ny5zZXRDb250ZW50KGh0bWxfYzZjNzM4NDgzZTM3NDQ4NWJiYjU3NjIyZjczOTJjOGIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNTBlYTVkZjE5OTkzNGI1NmJhNzE0OTA2ZjlhNjYzYjAuYmluZFBvcHVwKHBvcHVwX2ViMjliZTc2ZTc5NzQyMzY5ZTgwOWYyNDZmZTRiMjg3KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzFkNDkzMmE4ZDVhZjQ4NTY5MDUzMmZlNjIzZmNkYzk3ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjQ3OTI2NzAwMDAwMDA2LC03OS40MTk3NDk3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICJyZWQiLAogICJmaWxsT3BhY2l0eSI6IDAuMiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDQsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNTdiOWNhZDZhNzk2NDMxZmIwOWJhNTM1Y2NmZTg5ZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMWViYmZiYjQzYzUxNDMzNjkwZjQ3NTkzNzcxOTk2YWEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZWZjMjFiZDljZDQ5NDAyMmJlMTY0NDJhYTNjZjBjM2QgPSAkKCc8ZGl2IGlkPSJodG1sX2VmYzIxYmQ5Y2Q0OTQwMjJiZTE2NDQyYWEzY2YwYzNkIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5MaXR0bGUgUG9ydHVnYWwsVHJpbml0eSwgV2VzdCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8xZWJiZmJiNDNjNTE0MzM2OTBmNDc1OTM3NzE5OTZhYS5zZXRDb250ZW50KGh0bWxfZWZjMjFiZDljZDQ5NDAyMmJlMTY0NDJhYTNjZjBjM2QpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMWQ0OTMyYThkNWFmNDg1NjkwNTMyZmU2MjNmY2RjOTcuYmluZFBvcHVwKHBvcHVwXzFlYmJmYmI0M2M1MTQzMzY5MGY0NzU5Mzc3MTk5NmFhKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzllNmNlNTU0NjRiMTQ3Zjg5YTdjYTA4ZjU1NThiMGE2ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjc5NTU3MSwtNzkuMzUyMTg4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICJyZWQiLAogICJmaWxsT3BhY2l0eSI6IDAuMiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDQsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNTdiOWNhZDZhNzk2NDMxZmIwOWJhNTM1Y2NmZTg5ZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMWZhNDBhY2I0NTJmNGIzNzhlNGQ0ZWJhOGU1NGI0YTEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZmIxZGZmMzgxMWIyNDI2NDkwNDI5OGZlYzNmMDg2MzEgPSAkKCc8ZGl2IGlkPSJodG1sX2ZiMWRmZjM4MTFiMjQyNjQ5MDQyOThmZWMzZjA4NjMxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5UaGUgRGFuZm9ydGggV2VzdCxSaXZlcmRhbGUsIEVhc3QgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMWZhNDBhY2I0NTJmNGIzNzhlNGQ0ZWJhOGU1NGI0YTEuc2V0Q29udGVudChodG1sX2ZiMWRmZjM4MTFiMjQyNjQ5MDQyOThmZWMzZjA4NjMxKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzllNmNlNTU0NjRiMTQ3Zjg5YTdjYTA4ZjU1NThiMGE2LmJpbmRQb3B1cChwb3B1cF8xZmE0MGFjYjQ1MmY0YjM3OGU0ZDRlYmE4ZTU0YjRhMSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8yMGY3NWM2OGRhMTM0NDQxODVmMDFhYjE5Zjc5MjJkZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY0NzE3NjgsLTc5LjM4MTU3NjQwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICJyZWQiLAogICJmaWxsT3BhY2l0eSI6IDAuMiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDQsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNTdiOWNhZDZhNzk2NDMxZmIwOWJhNTM1Y2NmZTg5ZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZjExNGI3OTA4MTYwNGJkZDhmZDAxMDRkYzkyZTViYmYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNDBlMDFhZTQ3ZmQ2NDgzYjk2ODUxZTc4MTRhOWY4YzMgPSAkKCc8ZGl2IGlkPSJodG1sXzQwZTAxYWU0N2ZkNjQ4M2I5Njg1MWU3ODE0YTlmOGMzIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5EZXNpZ24gRXhjaGFuZ2UsVG9yb250byBEb21pbmlvbiBDZW50cmUsIERvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2YxMTRiNzkwODE2MDRiZGQ4ZmQwMTA0ZGM5MmU1YmJmLnNldENvbnRlbnQoaHRtbF80MGUwMWFlNDdmZDY0ODNiOTY4NTFlNzgxNGE5ZjhjMyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8yMGY3NWM2OGRhMTM0NDQxODVmMDFhYjE5Zjc5MjJkZS5iaW5kUG9wdXAocG9wdXBfZjExNGI3OTA4MTYwNGJkZDhmZDAxMDRkYzkyZTViYmYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNzgwNDUwYzQzNThkNDYzZDk3OTJjNDMzZGY4ZTAyN2IgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42MzY4NDcyLC03OS40MjgxOTE0MDAwMDAwMl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAicmVkIiwKICAiZmlsbE9wYWNpdHkiOiAwLjIsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA0LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzU3YjljYWQ2YTc5NjQzMWZiMDliYTUzNWNjZmU4OWQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzNhYTI4YzU3ODk5ZjQ5NDM4OWQ1MmI1NmExZjE2ZTdjID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzk2ZjkwMjQxZTU5ZjQ2MDU5MDhkMDk0NjM1M2FhNjIyID0gJCgnPGRpdiBpZD0iaHRtbF85NmY5MDI0MWU1OWY0NjA1OTA4ZDA5NDYzNTNhYTYyMiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+QnJvY2t0b24sRXhoaWJpdGlvbiBQbGFjZSxQYXJrZGFsZSBWaWxsYWdlLCBXZXN0IFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzNhYTI4YzU3ODk5ZjQ5NDM4OWQ1MmI1NmExZjE2ZTdjLnNldENvbnRlbnQoaHRtbF85NmY5MDI0MWU1OWY0NjA1OTA4ZDA5NDYzNTNhYTYyMik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl83ODA0NTBjNDM1OGQ0NjNkOTc5MmM0MzNkZjhlMDI3Yi5iaW5kUG9wdXAocG9wdXBfM2FhMjhjNTc4OTlmNDk0Mzg5ZDUyYjU2YTFmMTZlN2MpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOTAwNWFhMzgwYTUzNDBlYTliYmM5YWFjM2QwN2Y3YjIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Njg5OTg1LC03OS4zMTU1NzE1OTk5OTk5OF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAicmVkIiwKICAiZmlsbE9wYWNpdHkiOiAwLjIsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA0LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzU3YjljYWQ2YTc5NjQzMWZiMDliYTUzNWNjZmU4OWQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2ExYjFmNTAwNDBiODRkMGI5NjNhN2ZlZWQzODhkZjkyID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzZlMWJhZWIxZjU2ZTQwY2ZiMDU2YTA2Nzk4YzRlMDZiID0gJCgnPGRpdiBpZD0iaHRtbF82ZTFiYWViMWY1NmU0MGNmYjA1NmEwNjc5OGM0ZTA2YiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+VGhlIEJlYWNoZXMgV2VzdCxJbmRpYSBCYXphYXIsIEVhc3QgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYTFiMWY1MDA0MGI4NGQwYjk2M2E3ZmVlZDM4OGRmOTIuc2V0Q29udGVudChodG1sXzZlMWJhZWIxZjU2ZTQwY2ZiMDU2YTA2Nzk4YzRlMDZiKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzkwMDVhYTM4MGE1MzQwZWE5YmJjOWFhYzNkMDdmN2IyLmJpbmRQb3B1cChwb3B1cF9hMWIxZjUwMDQwYjg0ZDBiOTYzYTdmZWVkMzg4ZGY5Mik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl82MmEyNGJlZjEyZjE0YjQ0YWQ3MmNiNTU4YzEwY2ViNSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY0ODE5ODUsLTc5LjM3OTgxNjkwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICJyZWQiLAogICJmaWxsT3BhY2l0eSI6IDAuMiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDQsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNTdiOWNhZDZhNzk2NDMxZmIwOWJhNTM1Y2NmZTg5ZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMDhlYjNhZjBlN2RiNDkxMTliNDRhNThjZTg0YTAwNDggPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMTMwZDliOWRiMGYyNDY3NWExOWRlOTExMTkxMTQ4ZWYgPSAkKCc8ZGl2IGlkPSJodG1sXzEzMGQ5YjlkYjBmMjQ2NzVhMTlkZTkxMTE5MTE0OGVmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Db21tZXJjZSBDb3VydCxWaWN0b3JpYSBIb3RlbCwgRG93bnRvd24gVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMDhlYjNhZjBlN2RiNDkxMTliNDRhNThjZTg0YTAwNDguc2V0Q29udGVudChodG1sXzEzMGQ5YjlkYjBmMjQ2NzVhMTlkZTkxMTE5MTE0OGVmKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzYyYTI0YmVmMTJmMTRiNDRhZDcyY2I1NThjMTBjZWI1LmJpbmRQb3B1cChwb3B1cF8wOGViM2FmMGU3ZGI0OTExOWI0NGE1OGNlODRhMDA0OCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl80MDU2NmRjZjkwZmU0NTMxYTA1MTY4NjQ1ZDYyZTNkNSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1OTUyNTUsLTc5LjM0MDkyM10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAicmVkIiwKICAiZmlsbE9wYWNpdHkiOiAwLjIsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA0LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzU3YjljYWQ2YTc5NjQzMWZiMDliYTUzNWNjZmU4OWQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2I3Nzg4OGRhYjM4MTQ0Zjk5MGYwZjNjZDM4MDg5MDMwID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2UyZjBlM2RlMjc1NjQ1YTFiZDg4ZmMwMzBlNjc1YTk2ID0gJCgnPGRpdiBpZD0iaHRtbF9lMmYwZTNkZTI3NTY0NWExYmQ4OGZjMDMwZTY3NWE5NiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U3R1ZGlvIERpc3RyaWN0LCBFYXN0IFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2I3Nzg4OGRhYjM4MTQ0Zjk5MGYwZjNjZDM4MDg5MDMwLnNldENvbnRlbnQoaHRtbF9lMmYwZTNkZTI3NTY0NWExYmQ4OGZjMDMwZTY3NWE5Nik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl80MDU2NmRjZjkwZmU0NTMxYTA1MTY4NjQ1ZDYyZTNkNS5iaW5kUG9wdXAocG9wdXBfYjc3ODg4ZGFiMzgxNDRmOTkwZjBmM2NkMzgwODkwMzApOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNDRlZmM1ZjhmZmM3NDZjZDk3MWViNTAwYzZlOGNiY2IgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MjgwMjA1LC03OS4zODg3OTAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICJyZWQiLAogICJmaWxsT3BhY2l0eSI6IDAuMiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDQsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNTdiOWNhZDZhNzk2NDMxZmIwOWJhNTM1Y2NmZTg5ZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfN2RjNGJkYmMwMjdlNDlkNGE0ZjkwMTYzZjFlYjU0NGUgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMzFjZTA0NWE5MzU0NGRkNmI0ODM1NWU0OTQ3OWUzNWQgPSAkKCc8ZGl2IGlkPSJodG1sXzMxY2UwNDVhOTM1NDRkZDZiNDgzNTVlNDk0NzllMzVkIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5MYXdyZW5jZSBQYXJrLCBDZW50cmFsIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzdkYzRiZGJjMDI3ZTQ5ZDRhNGY5MDE2M2YxZWI1NDRlLnNldENvbnRlbnQoaHRtbF8zMWNlMDQ1YTkzNTQ0ZGQ2YjQ4MzU1ZTQ5NDc5ZTM1ZCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl80NGVmYzVmOGZmYzc0NmNkOTcxZWI1MDBjNmU4Y2JjYi5iaW5kUG9wdXAocG9wdXBfN2RjNGJkYmMwMjdlNDlkNGE0ZjkwMTYzZjFlYjU0NGUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMjc5ZGFiNmM1MDE1NGFiN2E5OThiNmNjYjdiZWNhODIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MTE2OTQ4LC03OS40MTY5MzU1OTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAicmVkIiwKICAiZmlsbE9wYWNpdHkiOiAwLjIsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA0LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzU3YjljYWQ2YTc5NjQzMWZiMDliYTUzNWNjZmU4OWQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2ViYzAwYTM3MTU5ODQ1N2U4MmUzOWI2ZTdlY2M0YTEyID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzdhY2E2NWM5MWE0OTQ5YWRhZjExYjZlMDIzMzUxNWQ2ID0gJCgnPGRpdiBpZD0iaHRtbF83YWNhNjVjOTFhNDk0OWFkYWYxMWI2ZTAyMzM1MTVkNiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Um9zZWxhd24sIENlbnRyYWwgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZWJjMDBhMzcxNTk4NDU3ZTgyZTM5YjZlN2VjYzRhMTIuc2V0Q29udGVudChodG1sXzdhY2E2NWM5MWE0OTQ5YWRhZjExYjZlMDIzMzUxNWQ2KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzI3OWRhYjZjNTAxNTRhYjdhOTk4YjZjY2I3YmVjYTgyLmJpbmRQb3B1cChwb3B1cF9lYmMwMGEzNzE1OTg0NTdlODJlMzliNmU3ZWNjNGExMik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wNGY5Yjg3NWNkN2Y0NjFhOGY3MDMzYTcwNWQ1OWNlMiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcxMjc1MTEsLTc5LjM5MDE5NzVdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogInJlZCIsCiAgImZpbGxPcGFjaXR5IjogMC4yLAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF81N2I5Y2FkNmE3OTY0MzFmYjA5YmE1MzVjY2ZlODlkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF85ZGQ3YjQ3MTcwNGU0MWQyOGUxNGZmNGUwNzBmOGZjNSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9mYjJiNWNmMjNkNDc0Yjc5OTljM2E5MjZmMDk0N2M1OSA9ICQoJzxkaXYgaWQ9Imh0bWxfZmIyYjVjZjIzZDQ3NGI3OTk5YzNhOTI2ZjA5NDdjNTkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRhdmlzdmlsbGUgTm9ydGgsIENlbnRyYWwgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOWRkN2I0NzE3MDRlNDFkMjhlMTRmZjRlMDcwZjhmYzUuc2V0Q29udGVudChodG1sX2ZiMmI1Y2YyM2Q0NzRiNzk5OWMzYTkyNmYwOTQ3YzU5KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzA0ZjliODc1Y2Q3ZjQ2MWE4ZjcwMzNhNzA1ZDU5Y2UyLmJpbmRQb3B1cChwb3B1cF85ZGQ3YjQ3MTcwNGU0MWQyOGUxNGZmNGUwNzBmOGZjNSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9iYWQwOTYxMzM1MTM0YjdlYWM2MTdkMzEwZTdhNDNkNCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY5Njk0NzYsLTc5LjQxMTMwNzIwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICJyZWQiLAogICJmaWxsT3BhY2l0eSI6IDAuMiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDQsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNTdiOWNhZDZhNzk2NDMxZmIwOWJhNTM1Y2NmZTg5ZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYmZjZTkxMzdlOGY2NGQzMTk3ZjkzZjk4NmFjNGM4ZWYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMDBiMzZhYmMwYTMwNGI2ZGFjMTkwYWQ3MjVlZjY0MjcgPSAkKCc8ZGl2IGlkPSJodG1sXzAwYjM2YWJjMGEzMDRiNmRhYzE5MGFkNzI1ZWY2NDI3IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Gb3Jlc3QgSGlsbCBOb3J0aCxGb3Jlc3QgSGlsbCBXZXN0LCBDZW50cmFsIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2JmY2U5MTM3ZThmNjRkMzE5N2Y5M2Y5ODZhYzRjOGVmLnNldENvbnRlbnQoaHRtbF8wMGIzNmFiYzBhMzA0YjZkYWMxOTBhZDcyNWVmNjQyNyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9iYWQwOTYxMzM1MTM0YjdlYWM2MTdkMzEwZTdhNDNkNC5iaW5kUG9wdXAocG9wdXBfYmZjZTkxMzdlOGY2NGQzMTk3ZjkzZjk4NmFjNGM4ZWYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYmRlNjcyNGI1Y2VkNDZjNzgwODAxZGEyODRjYzM3NDYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NjE2MDgzLC03OS40NjQ3NjMyOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAicmVkIiwKICAiZmlsbE9wYWNpdHkiOiAwLjIsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA0LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzU3YjljYWQ2YTc5NjQzMWZiMDliYTUzNWNjZmU4OWQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2U0ZmFmNjg1MGNjNTQ5ODI5NTNkMmM3YmIxNzU2ODdjID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzY1ODllNjhkYjY3YjQ0NzA5OTc3MGVjMjU4MWI0MGVhID0gJCgnPGRpdiBpZD0iaHRtbF82NTg5ZTY4ZGI2N2I0NDcwOTk3NzBlYzI1ODFiNDBlYSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+SGlnaCBQYXJrLFRoZSBKdW5jdGlvbiBTb3V0aCwgV2VzdCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9lNGZhZjY4NTBjYzU0OTgyOTUzZDJjN2JiMTc1Njg3Yy5zZXRDb250ZW50KGh0bWxfNjU4OWU2OGRiNjdiNDQ3MDk5NzcwZWMyNTgxYjQwZWEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYmRlNjcyNGI1Y2VkNDZjNzgwODAxZGEyODRjYzM3NDYuYmluZFBvcHVwKHBvcHVwX2U0ZmFmNjg1MGNjNTQ5ODI5NTNkMmM3YmIxNzU2ODdjKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzQ5YmY0MmY1YTU4MzQ1MDQ4MThjZjk0N2MwYWJmODE2ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzE1MzgzNCwtNzkuNDA1Njc4NDAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogInJlZCIsCiAgImZpbGxPcGFjaXR5IjogMC4yLAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF81N2I5Y2FkNmE3OTY0MzFmYjA5YmE1MzVjY2ZlODlkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jNTk1Zjc1NWI1ZmU0ZjBjODAyNTEzNjI5YTVmYjlhNCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8xMjEzMjU0MjhmYzQ0ZjlhOGM3NGJmMzY1NzY1OWQzNSA9ICQoJzxkaXYgaWQ9Imh0bWxfMTIxMzI1NDI4ZmM0NGY5YThjNzRiZjM2NTc2NTlkMzUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5vcnRoIFRvcm9udG8gV2VzdCwgQ2VudHJhbCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9jNTk1Zjc1NWI1ZmU0ZjBjODAyNTEzNjI5YTVmYjlhNC5zZXRDb250ZW50KGh0bWxfMTIxMzI1NDI4ZmM0NGY5YThjNzRiZjM2NTc2NTlkMzUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNDliZjQyZjVhNTgzNDUwNDgxOGNmOTQ3YzBhYmY4MTYuYmluZFBvcHVwKHBvcHVwX2M1OTVmNzU1YjVmZTRmMGM4MDI1MTM2MjlhNWZiOWE0KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2U3MTc2ZGJiOGQ3MjQ4OTZiZTc3OGE1MDc2YTJjMjVlID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjcyNzA5NywtNzkuNDA1Njc4NDAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogInJlZCIsCiAgImZpbGxPcGFjaXR5IjogMC4yLAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF81N2I5Y2FkNmE3OTY0MzFmYjA5YmE1MzVjY2ZlODlkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8yZTZkZmIxNzc2MWI0ZTkzOTMxZTVmMjQwNTFhNzhlYyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF84Yjg0YjkzMjliMjg0YzQ0YWZkMjI3NDljYzIzZGM2ZCA9ICQoJzxkaXYgaWQ9Imh0bWxfOGI4NGI5MzI5YjI4NGM0NGFmZDIyNzQ5Y2MyM2RjNmQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlRoZSBBbm5leCxOb3J0aCBNaWR0b3duLFlvcmt2aWxsZSwgQ2VudHJhbCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8yZTZkZmIxNzc2MWI0ZTkzOTMxZTVmMjQwNTFhNzhlYy5zZXRDb250ZW50KGh0bWxfOGI4NGI5MzI5YjI4NGM0NGFmZDIyNzQ5Y2MyM2RjNmQpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZTcxNzZkYmI4ZDcyNDg5NmJlNzc4YTUwNzZhMmMyNWUuYmluZFBvcHVwKHBvcHVwXzJlNmRmYjE3NzYxYjRlOTM5MzFlNWYyNDA1MWE3OGVjKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzg5MDI3Zjc2ZjA3ZjQ1ZThiYjZlZWE2ZmE3ODA1ZDAwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjQ4OTU5NywtNzkuNDU2MzI1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICJyZWQiLAogICJmaWxsT3BhY2l0eSI6IDAuMiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDQsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNTdiOWNhZDZhNzk2NDMxZmIwOWJhNTM1Y2NmZTg5ZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNWQxZWQwYzZhODEwNDI5Mjg0MDliNTIwODQ4MThmZDEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYjE4OWM3N2I3MmM5NDY1NzhiMzBmNmIzN2JjYmQ5MTcgPSAkKCc8ZGl2IGlkPSJodG1sX2IxODljNzdiNzJjOTQ2NTc4YjMwZjZiMzdiY2JkOTE3IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5QYXJrZGFsZSxSb25jZXN2YWxsZXMsIFdlc3QgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNWQxZWQwYzZhODEwNDI5Mjg0MDliNTIwODQ4MThmZDEuc2V0Q29udGVudChodG1sX2IxODljNzdiNzJjOTQ2NTc4YjMwZjZiMzdiY2JkOTE3KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzg5MDI3Zjc2ZjA3ZjQ1ZThiYjZlZWE2ZmE3ODA1ZDAwLmJpbmRQb3B1cChwb3B1cF81ZDFlZDBjNmE4MTA0MjkyODQwOWI1MjA4NDgxOGZkMSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9mZjUzNzcyMmYxMGI0YjViODYwYjE5ZTg3ZmQwMGI2ZCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcwNDMyNDQsLTc5LjM4ODc5MDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogInJlZCIsCiAgImZpbGxPcGFjaXR5IjogMC4yLAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF81N2I5Y2FkNmE3OTY0MzFmYjA5YmE1MzVjY2ZlODlkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9hNGRjNDMzNTY1YzY0NjJmOTYzOTcwY2VhNGI5ZjJlMiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9lYmYyY2I0YTVjYmM0YjllOTJhZWE5NjE1OTBkZmU3ZCA9ICQoJzxkaXYgaWQ9Imh0bWxfZWJmMmNiNGE1Y2JjNGI5ZTkyYWVhOTYxNTkwZGZlN2QiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRhdmlzdmlsbGUsIENlbnRyYWwgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYTRkYzQzMzU2NWM2NDYyZjk2Mzk3MGNlYTRiOWYyZTIuc2V0Q29udGVudChodG1sX2ViZjJjYjRhNWNiYzRiOWU5MmFlYTk2MTU5MGRmZTdkKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2ZmNTM3NzIyZjEwYjRiNWI4NjBiMTllODdmZDAwYjZkLmJpbmRQb3B1cChwb3B1cF9hNGRjNDMzNTY1YzY0NjJmOTYzOTcwY2VhNGI5ZjJlMik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9mZGRkMGJiNjZiMjY0NjlkYWQyYzcxNTI1NDkzOWRkMSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY2MjY5NTYsLTc5LjQwMDA0OTNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogInJlZCIsCiAgImZpbGxPcGFjaXR5IjogMC4yLAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF81N2I5Y2FkNmE3OTY0MzFmYjA5YmE1MzVjY2ZlODlkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9kMmIzMTkxZjBiMmE0MmFjOTM1ZjViYjM3Y2MwZWJkMyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jYzg4NWViOWI4ZjQ0MmZkYjE5YjNiNjAzNDc5NDhjOSA9ICQoJzxkaXYgaWQ9Imh0bWxfY2M4ODVlYjliOGY0NDJmZGIxOWIzYjYwMzQ3OTQ4YzkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkhhcmJvcmQsVW5pdmVyc2l0eSBvZiBUb3JvbnRvLCBEb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9kMmIzMTkxZjBiMmE0MmFjOTM1ZjViYjM3Y2MwZWJkMy5zZXRDb250ZW50KGh0bWxfY2M4ODVlYjliOGY0NDJmZGIxOWIzYjYwMzQ3OTQ4YzkpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZmRkZDBiYjY2YjI2NDY5ZGFkMmM3MTUyNTQ5MzlkZDEuYmluZFBvcHVwKHBvcHVwX2QyYjMxOTFmMGIyYTQyYWM5MzVmNWJiMzdjYzBlYmQzKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2NkZWQ2YTlmYjIzODRlMmZiMDBjNTQwMGUzNjc4ODdjID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjUxNTcwNiwtNzkuNDg0NDQ5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAicmVkIiwKICAiZmlsbE9wYWNpdHkiOiAwLjIsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA0LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzU3YjljYWQ2YTc5NjQzMWZiMDliYTUzNWNjZmU4OWQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzU5ODlkNDUyZWZlMDRkZWI4NGM3ZjBlMTBiNGEzMWJiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzcwMGQ3MDFhYjViNzRmMmQ5YzNhMjM5YjhmZjExODc0ID0gJCgnPGRpdiBpZD0iaHRtbF83MDBkNzAxYWI1Yjc0ZjJkOWMzYTIzOWI4ZmYxMTg3NCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+UnVubnltZWRlLFN3YW5zZWEsIFdlc3QgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNTk4OWQ0NTJlZmUwNGRlYjg0YzdmMGUxMGI0YTMxYmIuc2V0Q29udGVudChodG1sXzcwMGQ3MDFhYjViNzRmMmQ5YzNhMjM5YjhmZjExODc0KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2NkZWQ2YTlmYjIzODRlMmZiMDBjNTQwMGUzNjc4ODdjLmJpbmRQb3B1cChwb3B1cF81OTg5ZDQ1MmVmZTA0ZGViODRjN2YwZTEwYjRhMzFiYik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wNGEwZjI5ZjVlOTI0N2YwYWFkODU3NmMwOGExNWVjZiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY4OTU3NDMsLTc5LjM4MzE1OTkwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICJyZWQiLAogICJmaWxsT3BhY2l0eSI6IDAuMiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDQsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNTdiOWNhZDZhNzk2NDMxZmIwOWJhNTM1Y2NmZTg5ZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZmM0YjIyMjBjMDVlNDE4YTk5NzAzNmJkYzA1NzA3OTIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfN2I2MTRiOWQzNmIyNDhlNWJmMWFkZjQ5ODIxNmUzMTcgPSAkKCc8ZGl2IGlkPSJodG1sXzdiNjE0YjlkMzZiMjQ4ZTViZjFhZGY0OTgyMTZlMzE3IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Nb29yZSBQYXJrLFN1bW1lcmhpbGwgRWFzdCwgQ2VudHJhbCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9mYzRiMjIyMGMwNWU0MThhOTk3MDM2YmRjMDU3MDc5Mi5zZXRDb250ZW50KGh0bWxfN2I2MTRiOWQzNmIyNDhlNWJmMWFkZjQ5ODIxNmUzMTcpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMDRhMGYyOWY1ZTkyNDdmMGFhZDg1NzZjMDhhMTVlY2YuYmluZFBvcHVwKHBvcHVwX2ZjNGIyMjIwYzA1ZTQxOGE5OTcwMzZiZGMwNTcwNzkyKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2RhMzNkODkyM2ZhMzQ2Yzg4MzMwNjQ1Y2VlYjliOGZlID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjUzMjA1NywtNzkuNDAwMDQ5M10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAicmVkIiwKICAiZmlsbE9wYWNpdHkiOiAwLjIsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA0LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzU3YjljYWQ2YTc5NjQzMWZiMDliYTUzNWNjZmU4OWQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2RlMWM1NzRkYzQzODQ1NjViMGY2MmQ1MDlhNDZiZWRlID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzdlYjExODhiN2UyMzRkNzhhZmZmZTdiYmFkYjg1MTJkID0gJCgnPGRpdiBpZD0iaHRtbF83ZWIxMTg4YjdlMjM0ZDc4YWZmZmU3YmJhZGI4NTEyZCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Q2hpbmF0b3duLEdyYW5nZSBQYXJrLEtlbnNpbmd0b24gTWFya2V0LCBEb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9kZTFjNTc0ZGM0Mzg0NTY1YjBmNjJkNTA5YTQ2YmVkZS5zZXRDb250ZW50KGh0bWxfN2ViMTE4OGI3ZTIzNGQ3OGFmZmZlN2JiYWRiODUxMmQpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZGEzM2Q4OTIzZmEzNDZjODgzMzA2NDVjZWViOWI4ZmUuYmluZFBvcHVwKHBvcHVwX2RlMWM1NzRkYzQzODQ1NjViMGY2MmQ1MDlhNDZiZWRlKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzk5MzIwMmVhNmFmYzQwOWZhNWIzNmFjYzg5ODA1N2Q1ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjg2NDEyMjk5OTk5OTksLTc5LjQwMDA0OTNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogInJlZCIsCiAgImZpbGxPcGFjaXR5IjogMC4yLAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF81N2I5Y2FkNmE3OTY0MzFmYjA5YmE1MzVjY2ZlODlkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8yNDA1Y2M2NmJhMzA0MmY1ODJkODI1NTNhYmE1ZTlmNyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9kZTEzNmQ5NjUxNGE0ZTgzYjJmZTY1YWZmY2RkMjMwYSA9ICQoJzxkaXYgaWQ9Imh0bWxfZGUxMzZkOTY1MTRhNGU4M2IyZmU2NWFmZmNkZDIzMGEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRlZXIgUGFyayxGb3Jlc3QgSGlsbCBTRSxSYXRobmVsbHksU291dGggSGlsbCxTdW1tZXJoaWxsIFdlc3QsIENlbnRyYWwgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMjQwNWNjNjZiYTMwNDJmNTgyZDgyNTUzYWJhNWU5Zjcuc2V0Q29udGVudChodG1sX2RlMTM2ZDk2NTE0YTRlODNiMmZlNjVhZmZjZGQyMzBhKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzk5MzIwMmVhNmFmYzQwOWZhNWIzNmFjYzg5ODA1N2Q1LmJpbmRQb3B1cChwb3B1cF8yNDA1Y2M2NmJhMzA0MmY1ODJkODI1NTNhYmE1ZTlmNyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8zYWQxMDQ0YTQ4MDM0NzkyYTEzZmI3M2NkNTdjMzBkMSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjYyODk0NjcsLTc5LjM5NDQxOTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogInJlZCIsCiAgImZpbGxPcGFjaXR5IjogMC4yLAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF81N2I5Y2FkNmE3OTY0MzFmYjA5YmE1MzVjY2ZlODlkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF82M2I3MGJmYzg2OTE0NmNmYTQ2MTBiYjk4YjE4NDM4OSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9iNTQ3MTY3N2E0NjA0MzU1YTk5MzA4ZmFlNjgxNGY2YiA9ICQoJzxkaXYgaWQ9Imh0bWxfYjU0NzE2NzdhNDYwNDM1NWE5OTMwOGZhZTY4MTRmNmIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNOIFRvd2VyLEJhdGh1cnN0IFF1YXksSXNsYW5kIGFpcnBvcnQsSGFyYm91cmZyb250IFdlc3QsS2luZyBhbmQgU3BhZGluYSxSYWlsd2F5IExhbmRzLFNvdXRoIE5pYWdhcmEsIERvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzYzYjcwYmZjODY5MTQ2Y2ZhNDYxMGJiOThiMTg0Mzg5LnNldENvbnRlbnQoaHRtbF9iNTQ3MTY3N2E0NjA0MzU1YTk5MzA4ZmFlNjgxNGY2Yik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8zYWQxMDQ0YTQ4MDM0NzkyYTEzZmI3M2NkNTdjMzBkMS5iaW5kUG9wdXAocG9wdXBfNjNiNzBiZmM4NjkxNDZjZmE0NjEwYmI5OGIxODQzODkpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZWQwMGRjY2FmYjkyNGFkNWE2NDMyYjJiM2ZmY2U5NzkgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Nzk1NjI2LC03OS4zNzc1Mjk0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAicmVkIiwKICAiZmlsbE9wYWNpdHkiOiAwLjIsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA0LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzU3YjljYWQ2YTc5NjQzMWZiMDliYTUzNWNjZmU4OWQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzIxMzM2NzA2YWRkNjRmZmI4ZDVmOThhNzBlMDkxZjRiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzA4MTM2MWVmMGFhYjRlNDZhMmIyMzAyMjNlMWE3YTE3ID0gJCgnPGRpdiBpZD0iaHRtbF8wODEzNjFlZjBhYWI0ZTQ2YTJiMjMwMjIzZTFhN2ExNyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Um9zZWRhbGUsIERvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzIxMzM2NzA2YWRkNjRmZmI4ZDVmOThhNzBlMDkxZjRiLnNldENvbnRlbnQoaHRtbF8wODEzNjFlZjBhYWI0ZTQ2YTJiMjMwMjIzZTFhN2ExNyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9lZDAwZGNjYWZiOTI0YWQ1YTY0MzJiMmIzZmZjZTk3OS5iaW5kUG9wdXAocG9wdXBfMjEzMzY3MDZhZGQ2NGZmYjhkNWY5OGE3MGUwOTFmNGIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYjlmMjcyM2JhZGY4NGJhNDhiMTg5YjUxNTY0OGZmOTMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDY0MzUyLC03OS4zNzQ4NDU5OTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAicmVkIiwKICAiZmlsbE9wYWNpdHkiOiAwLjIsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA0LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzU3YjljYWQ2YTc5NjQzMWZiMDliYTUzNWNjZmU4OWQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzFkNTQyYmY5YWRlMzQ5NWVhMzRmY2Y4ZWI4MDA5MmNlID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzY5ZWM5OGM2MGEyNTQzYmQ4ZGE3M2Y5ZWRhMGNhMmQ5ID0gJCgnPGRpdiBpZD0iaHRtbF82OWVjOThjNjBhMjU0M2JkOGRhNzNmOWVkYTBjYTJkOSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U3RuIEEgUE8gQm94ZXMgMjUgVGhlIEVzcGxhbmFkZSwgRG93bnRvd24gVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMWQ1NDJiZjlhZGUzNDk1ZWEzNGZjZjhlYjgwMDkyY2Uuc2V0Q29udGVudChodG1sXzY5ZWM5OGM2MGEyNTQzYmQ4ZGE3M2Y5ZWRhMGNhMmQ5KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2I5ZjI3MjNiYWRmODRiYTQ4YjE4OWI1MTU2NDhmZjkzLmJpbmRQb3B1cChwb3B1cF8xZDU0MmJmOWFkZTM0OTVlYTM0ZmNmOGViODAwOTJjZSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9kYjAxYWJmMTliZjM0YzYwOWYwZTgxNDZmMzQ0MDlhNiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY2Nzk2NywtNzkuMzY3Njc1M10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAicmVkIiwKICAiZmlsbE9wYWNpdHkiOiAwLjIsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA0LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzU3YjljYWQ2YTc5NjQzMWZiMDliYTUzNWNjZmU4OWQwKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzhkMGU3NWUyY2E3OTQwNTBiOGQyMGQwNDc0MGQ4NGVlID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzQ5OGQxMDIxYjljZTRkMjhiMDY1ZTE0YzdlYTQ2ZDk0ID0gJCgnPGRpdiBpZD0iaHRtbF80OThkMTAyMWI5Y2U0ZDI4YjA2NWUxNGM3ZWE0NmQ5NCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Q2FiYmFnZXRvd24sU3QuIEphbWVzIFRvd24sIERvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzhkMGU3NWUyY2E3OTQwNTBiOGQyMGQwNDc0MGQ4NGVlLnNldENvbnRlbnQoaHRtbF80OThkMTAyMWI5Y2U0ZDI4YjA2NWUxNGM3ZWE0NmQ5NCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9kYjAxYWJmMTliZjM0YzYwOWYwZTgxNDZmMzQ0MDlhNi5iaW5kUG9wdXAocG9wdXBfOGQwZTc1ZTJjYTc5NDA1MGI4ZDIwZDA0NzQwZDg0ZWUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYjQ5YzkyNzM0ZDY5NGFlNmE4YmJiZGU5ZDk4MmM5MmUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDg0MjkyLC03OS4zODIyODAyXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICJyZWQiLAogICJmaWxsT3BhY2l0eSI6IDAuMiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDQsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNTdiOWNhZDZhNzk2NDMxZmIwOWJhNTM1Y2NmZTg5ZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYTlmNjA3ZTYzN2U3NGE0MjljYjgwOThiMjJhNDM2YjEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfM2VmMzk2ZDIxYjM1NDIxZjhlZjBmZTZmODVjODVmMzkgPSAkKCc8ZGl2IGlkPSJodG1sXzNlZjM5NmQyMWIzNTQyMWY4ZWYwZmU2Zjg1Yzg1ZjM5IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5GaXJzdCBDYW5hZGlhbiBQbGFjZSxVbmRlcmdyb3VuZCBjaXR5LCBEb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9hOWY2MDdlNjM3ZTc0YTQyOWNiODA5OGIyMmE0MzZiMS5zZXRDb250ZW50KGh0bWxfM2VmMzk2ZDIxYjM1NDIxZjhlZjBmZTZmODVjODVmMzkpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYjQ5YzkyNzM0ZDY5NGFlNmE4YmJiZGU5ZDk4MmM5MmUuYmluZFBvcHVwKHBvcHVwX2E5ZjYwN2U2MzdlNzRhNDI5Y2I4MDk4YjIyYTQzNmIxKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzQ4M2JiMWY5ZjQ1YTRjNjZiNTJmYTgyMGYwYzVlZWE1ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjY1ODU5OSwtNzkuMzgzMTU5OTAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogInJlZCIsCiAgImZpbGxPcGFjaXR5IjogMC4yLAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNCwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF81N2I5Y2FkNmE3OTY0MzFmYjA5YmE1MzVjY2ZlODlkMCk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF81MDZhN2FiZmYzMjQ0NTQyOTg1ODlkNGJlYzJhMzNjNyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF82NTE4OTcwZDIyM2Y0OTBiYThlNGQ5M2MxMGU5M2M0ZSA9ICQoJzxkaXYgaWQ9Imh0bWxfNjUxODk3MGQyMjNmNDkwYmE4ZTRkOTNjMTBlOTNjNGUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNodXJjaCBhbmQgV2VsbGVzbGV5LCBEb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81MDZhN2FiZmYzMjQ0NTQyOTg1ODlkNGJlYzJhMzNjNy5zZXRDb250ZW50KGh0bWxfNjUxODk3MGQyMjNmNDkwYmE4ZTRkOTNjMTBlOTNjNGUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNDgzYmIxZjlmNDVhNGM2NmI1MmZhODIwZjBjNWVlYTUuYmluZFBvcHVwKHBvcHVwXzUwNmE3YWJmZjMyNDQ1NDI5ODU4OWQ0YmVjMmEzM2M3KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzk2YzE0OWQ4NjhiNjQ4NTJiZDYyNWU5ZjNiYThiMzU3ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjYyNzQzOSwtNzkuMzIxNTU4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICJyZWQiLAogICJmaWxsT3BhY2l0eSI6IDAuMiwKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDQsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNTdiOWNhZDZhNzk2NDMxZmIwOWJhNTM1Y2NmZTg5ZDApOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfODY5OTI0OGJlYmMzNDk0YWFlZTQzMmY2OGM4MjMyN2YgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfODI5N2QzYzEwM2MwNDFiZGI3OTAyN2FlMjk2YzMzMDQgPSAkKCc8ZGl2IGlkPSJodG1sXzgyOTdkM2MxMDNjMDQxYmRiNzkwMjdhZTI5NmMzMzA0IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5CdXNpbmVzcyBSZXBseSBNYWlsIFByb2Nlc3NpbmcgQ2VudHJlIDk2OSBFYXN0ZXJuLCBFYXN0IFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzg2OTkyNDhiZWJjMzQ5NGFhZWU0MzJmNjhjODIzMjdmLnNldENvbnRlbnQoaHRtbF84Mjk3ZDNjMTAzYzA0MWJkYjc5MDI3YWUyOTZjMzMwNCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85NmMxNDlkODY4YjY0ODUyYmQ2MjVlOWYzYmE4YjM1Ny5iaW5kUG9wdXAocG9wdXBfODY5OTI0OGJlYmMzNDk0YWFlZTQzMmY2OGM4MjMyN2YpOwoKICAgICAgICAgICAgCiAgICAgICAgCjwvc2NyaXB0Pg==" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>


