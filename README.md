
## Chandrayaan 2 orbit animation

This project holds the source code for the 3D and 2D animations used
in http://sankara.net/chandrayaan2.html. That page shows an animation
of the orbit of the ISRO <a href="http://www.isro.org/mars/home.aspx">
Chandrayaan 2</a> mission.

![Screenshot](/screenshots/chandrayaan2.png?raw=true)

## Features

I created this animation for educational purposes. It has the following features:

* Real-world orbit data and predictions based on information available from JPL/NASA HORIZONS interface
* Rendering of the orbit in 2D and 3D
* Rendering of the orbit with either Earth or Moon at the center
* Rendering of the orbit with views locked on Earth, Moon, or the spacecraft
* Views aligned with J2000 reference axes
* Information on all earth bound and moon bound maneuvers (engine burns)
* Realistic textures for Earth and Moon in 3D mode
* Astronomically correct rendering of sunlight on Earth and Moon, poles, and polar axes
* Various animation controls for education - camera controls (pan, zoom, rotate), timeline controls, visibility controls
* A Joy Ride feature which lets you fly along with Chandrayaan 2 (video capture: https://www.youtube.com/watch?v=go-vquqMZdk)
    
## Design

## High level design

The animation has 2D and 3D rendering modes. 

The 2D mode uses SVG and D3 JS. Planetary orbits are rendered as ellipses
based on orbital elements. Spacecraft orbits are rendered using line segments
using position data.

The 3D mode uses THREE JS.

JQuery and JQueryUI are used for control and information panels.

Orbit data is fetched offline from JPL/NASA HORIZONS.
This data in CSV format is processed a bit and converted into JSON format 
for use in the animation. A few astronomy functions are based on Steve Moshier's routines.

### Fetching orbit data

The Perl script orbits.pl is used to fetch orbit data during development time from
<a href="http://ssd.jpl.nasa.gov/?horizons">NASA JPL HORIZONS</a> web interface.

The script supports the following options:

    --phase=[geo|lunar|lro]   # geocentric or selenocentric phase -- defaults to geo
    --data-dir=<datadir>      # place to save orbit data files -- defaults to .
    --use-cache               # use orbit data retrieved and saved earlier -- optional

Raw orbit data obtained from JPL is stored into the following files:

    ho-<id>-elements.txt  # orbital elements for one instant of time
    ho-<id>-vectors.txt   # co-ordinates for a period of time

Orbital elements are also stored here (though they aren't used at present):

    ho-<id>-orbit.txt     # orbital elements for one instant of time

Orbit data for use by the JavaScript is written in JSON format in a time-stamped directory under data-fetched:

    geo-cy2.json                # contains all geocentric orbit data (elements and vectors) until about 2019-09-10
    lunar-cy2.json              # contains all selenocentric orbit data (elements and vectors) until about 2019-09-10
    lunar-lro.json              # contains all selenocentric orbit data for Sep and Oct 2019 - post Vikram Loss of Signal

### Web page

The site consists of the following three sets of files:

#### Core project files

    chandrayaan2.html         # HTML page
    cy2.js                    # JavaScript handling animation
    astro.js                  # A few astronomy support functions
    cy2.css                   # CSS for the web page
    whatsnew-cy2.html         # What's new page
    geo-cy2.json              # contains all geocentric orbit data
    lunary-cy2.json           # contains all selenocentric orbit data

#### Third party library files, style sheets, and images

    d3.v3.min.js
    ephemeris-0.1.0.min.js (https://github.com/mivion/ephemeris)
    jquery.dialogextend.min.js
    jquery-ui-1.10.3.custom.min.js
    jquery-1.9.1.js
    three.min.js
    TrackballControls.js
    css/ui-darkness/images/*
    css/ui-darkness/*.css
    images/* (earth and moon textues for a few sources)

#### Analytics

    ga.js                 # Google analytics

### Hosting

At present the page can be hosted statically. There are no server components needed.
However, to prevent browsers from complaining about CORS, one may use a tiny web server
like Mongoose to test the local site. 

## Credits

* Jon D. Giorgini for helping with the JPL/HORIZONS interface and data. 
  He was very responsive whenever I mailed him my queries.
  He has been of great help since 2013 for the Mars Orbiter Mission until now
  for the Chandrayaan 2 mission.
  
* Members of the Bangalore Astronomy Society (http://bas.org.in/) for their valuable feedback

* Members of the Reddit r/isro (https://www.reddit.com/r/ISRO/) community for their valuable feedback
  
## Future work

The code base needs a rewrite. The very first release was for the Mars Orbiter Mission launch in 2013. 
Minor changes were made later to support MOM Mars orbit insertion and the Pluto flyby of New Horizons.

After a gap of 6 years, this has been modified again in 2019 to support the Chandrayaan 2 mission. 
The major changes were for 3D support. In that process, the code quality has degraded.

The rewrite will focus on present-day JavaScript tooling, better abstraction, 
better separation of concerns (2D vs. 3D, model vs. rendering, etc.), extensibility
(how does one extend the code for a new mission easily merely by changing configurations), 
performance (decrease the load time; improve rendering smoothness; on-demand loading of high resolution
LRO textures), and responsive UX. The current UX is almost unusable on mobile screens. 

## Inspirations

* https://mgvez.github.io/jsorrery/ 
* https://github.com/Flowm/satvis
* https://github.com/CoryG89/MoonDemo 
* http://stuffin.space/ 
* https://theskylive.com/3dsolarsystem 


