---
layout: post
title:      "CLI Data Project: Cleaning, Cutting, Plating, Presenting"
date:       2017-10-15 14:03:47 -0400
permalink:  cli_data_project_cleaning_cutting_plating_presenting
---

----------------------------------------

WARNING!!! This is my first blog post. If it's a little boring and clunky, I promise they will get better. I appreciate your understanding in advance. And now....
---------------------------------------

"The competent programmer is fully aware of the strictly limited size of his own skull; therefore he approaches the programming task in full humility, and among other things he avoids clever tricks like the plague." -Edsger Dijkstra

Ahh, the CLI Data Project. When I first read about it I had some pretty grandiose ideas. I had looked at a lot of web-pages code using developer tools. The scraping labs (complete with static webpages designed by our teachers) hadn't been TOO difficult. Challenging, sure, but nothing crazy. After completing the kickstarter lab, watching the Woot|Meh Scraper lecture, and importing files in the MusicLibraryImporter lab, this seemed like it would be a fairly simple project

Leading up to this lab, I started thinking about what types of websites would be interesting to scrape. I settled on movie website. My family are pretty avid movie fans. I thought it would be fun to scrape a web page from one of the national movie sites (Fandango, MovieTickets, etc.) and maybe access links to local theaters from there. I envisioned a CLI that could display all of the movie data that the user was interested in and display it in a variety of formats (movie listing, locations, summaries, rotten tomato score, etc.). 

Of course that didn't happen. Please see the starting quote. 

I realized quickly that my initial project was both out of my league and of too large a scope for this project. I started by trying to scrape Fandango. While I was able to access the main page data, I was completely unable to scrape any of their secondary pages. I looked at workarounds on this for a few hours. I googled a variety of solutions. Apparently, Fandango doesn't like unlicensed people accessing their API. So Fandango was out.

I looked at several more national sites, and came across different varieties of the same problem. Either the site was explicitly denying me scrape access, or I just didn't know enough to work with what they did have. Ultimately I went to the site of my local arthouse theater. The site is written in basic HTML5 with no javascript. Eureka! 

My program runs in the CLI. It displays the basic informationa about the current films out, and gives the user the option to find more information or look at coming attractions.
-----------------------------

the program is set up with three classes:
1. Scraper, used to scrape and organize the data from the site
2. Movies, used to manipuate the data and instantiate Objects
3. CLI, which interfaces with Movies and creates the user input

**The Classes**
SCRAPER Class
After settling on this site, the scraping was easy. I used HTTParty to pull in the HTML, and Nokogiri to parse it. The website is split between the main page with current features, and a second page with the coming attraction. I set up a Scraper class with two separate class methods to scrape the data;
```
  def self.feature_scraper
    html = "http://www.dedhamcommunitytheatre.com/"
    data = Nokogiri::HTML(open(html))
    data
  end

  def self.ca_scraper # Coming Attractions
    data = self.feature_scraper
    ca_html = data.css("div.show p.soon + a").attribute('href').value
    ca_data = Nokogiri::HTML(open(ca_html))
  end
```
The link to the coming attractions is set within the main page. #self.scraper pulls the data from #feature_scraper, finds the url for the 'Coming Soon' page, and creates a Nokogiri parser from that new page.

MOVIES Class

I started by creating the following attr_accessors. This covers all of the data that a movie instance might need

```
attr_accessor :title, :starring, :type, :rating, :summary, :release_date, :times
```

Because of formatting differences, I created two methods within Movies to instantiate the movie objects: #self.create_features and #self.create_ca
Using Nokogiri's #css method, I was able to find and pull all the data needed to put proper attributes with values into the objects. You can look at all the code here: https://github.com/nmhalloran/movie_finder-cli-app/tree/master/movie_finder/lib/movie_finder

CLI Class

The CLI was the first part of the program created. I started out by stubbing out all of the interface that I wanted with hard-code. Then I set up the bin files. Once that was up and running I worked backwards to start finding the actual data and then replacing the hard code with actual scraped code.

the lib/movie_finder file holds all of the required files and gems, and bin/movie_finder grabs the rest of the bundling information and executes the file. 

**Challenges**

Finishing this project was very satisfying. The greatest challenge was not the actual programming. There were of course many instances where I did not know how to do something and had to look it up. It's called learning. For the most part however, the skill set needed had already been taught. The real challenge was realizing what my limitations were, and learning how to think like a coder. After every project I'm realizing more and more how important a few skills are:
1.) Realizing what you don't know.
2.) Identifying and setting up the scope of your project.
3.) Summarizing and outlining your project in plain English.
4.) Identifying what gems you might need.
5.) And for me after being out of college for a while getting back into constantly being in a 'learner' mindset, instead of an 'expert' mindset

Overall, I had a lot of fun with this project, and completed a lot of firsts: first program from scratch, first screencast, first blog, etc. Very excited to continue my learning journey!

Signing out,

Nick Halloran








