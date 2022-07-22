# Title Slide (2)

Hi everyone! Thank you so much for coming to our session today. I'm really 
excited to talk to you all today about how we're using AI and ML to help New 
York State hit its net zero carbon goals.

# Slide 3

Before I get started I should probably introduce myself!

My name is Mike Mahoney, and I'm currently a PhD student at the State University 
of New York College of Environmental Science and Forestry, as well as one of 
this year's ESIP community fellows working with the Machine Learning cluster. 
My work focuses on ways to help people understand large-scale systems, 
especially when it comes to land use and forest ecology. 
And the work I'm talking about today is all a part of that,
through my role with ESF's Climate and Applied Forest Research Institute.

# Slide 4

So, back in 2019, New York State signed its landmark climate bill, the 
Climate Leadership and Community Protection Act into law.
The bill does a lot of things, but in particular it 
mandates that the state needs to hit net zero emissions statewide by 2050.
In order to meet that goal, New York is working on reducing its greenhouse
gas emissions across all sectors, but that's only going to get us about 85% of
the way there. The last component of hitting net zero is going to require 
managing land across the state to capture and sequester carbon for years to come.
In particular, we're going to really depend on forest lands to work as 
carbon sinks. 

# Slide 5

But in order to rely on our forest lands, we need to know how much carbon is 
actually being captured by forests across the state. We also need a way
to monitor that carbon storage over time, and to prioritize any efforts by the
state to conserve and reforest land in order to capture more carbon.
But up until recently, we've had very limited ability to actually answer these
questions. 

# Slide 6

You see, our main insight into the state of New York forests comes from the 
USDA's Forest Inventory and Analysis program, or FIA for short. The FIA has
over 6,000 plots across New York State which we use to get point estimates
of forest structure across the state. And the FIA is a fantastic, irreplacable 
resource -- it's a national forest inventory that's been going 
strong in one form or another for almost a century. But just like any data 
source, it has some challenges. 

For instance, the FIA really only measures interior forests, and doesn't go out
and measure the sort of marginal forests and mixed use landscapes that make up
a lot of New York. They also measure relatively small plots with large
spaces between them, which means that FIA estimates have to be pretty 
broad-scale, which makes them hard to use to identify important areas for 
conservation or reforestation. Each plot is also only measured once every five
years, making it hard to see short-term changes in the landscape.

And again, FIA is pretty much best-in-class for measuring forests;
it's prohibitively expensive to go out and measure a more dense forest 
inventory.

# Slide 7

And this problem brings to mind that old Peter Drucker quote: if you can't 
measure something, you can't manage it. If New York wants to manage its lands
to help it reach net zero carbon, we need to find a better way to track how
much carbon is being stored in our land.

And that's where our work comes in. The core question of our work is: how can we
use remote sensing data and machine learning to try and fill in those measurement
gaps?

# Slide 8

On the remote sensing end of the question, one thing that New York has going for
it is that we have a good amount of high-resolution airborne LiDAR available for
us to work with. In particular, since 2014 a number of groups -- especially FEMA
and the USGS -- have produced high-quality publicly available LiDAR data sets
that we're able to take advantage of to try and get a sense of forest structure
across the state. Of course, these data sets don't all represent the same set
of conditions -- a lot can change in six years -- and there's a lot of the state 
that we don't have this data for at all.

# Slide 9

And so what we do in our group is take advantage of this LiDAR where it exists to
generate point-in-time snapshots of New York's forest lands, and then use 
models fit on spaceborne remote sensing data, particularly from Landsat, to 
extrapolate beyond those spatiotemporal extents.

And so what I want to focus on today is to talk about how we've applied that
approach to those key questions I mentioned, to try and track how much
carbon is being captured by forests across the state and to try to prioritize
conservation efforts.

# Slide 10

So, I want to start off by talking about how we track how much carbon is stored
in forests across the state. 

# Slide 11

And to reiterate, the FIA is hands down our best resource for figuring out
how much carbon is stored in any given area. Now, the way FIA lays out their
plots is a little bit wonky -- a given "plot" is actually a set of four 
disconnected "subplots", which on this diagram are the circles with the lines 
connecting their centers. Inside those subplot boundaries, the FIA measures each 
tree and records its height and diameter; through some standard equations, we 
can then estimate the actual biomass of the tree, which can then be converted 
into carbon storage.

# Slide 12

What we do is take those plot-level biomass estimates and associate them with
the airborne LiDAR point clouds. We then calculate a number of metrics from 
the LiDAR point clouds, and use them to fit machine learning models.

# Slide 13

Specifically, we fit a random forest, gradient boosting machine, and support 
vector machine to the data from these FIA plots. We then use each of these models to 
predict a validation data set, then use those predictions to fit a simple
linear regression model, combining the machine learning models into a single
prediction. That ensembling step gives us a lot of freedom to make changes to
the individual machine learning methods -- if we accidentally make one model
much less predictive, it will get downweighted in the ensemble and the other two
models will pick up the slack. We also consistently see that the ensemble model
is more accurate than any of the individual component models.

# Slide 14

And so we then use that final ensemble model to predict biomass
on a 30 meter grid for all the years and areas that we have LiDAR data!

# Slide 15

Now, I'm not going to dive too deeply into our assessment protocol for these 
maps, but I do want to highlight that we make a point of assessing our map 
accuracy aggregated up to multiple scales. And what we find is that our 
predictions are pretty accurate even at the finest scales we investigate, with 
R squared ranging from 76 to 81 percent. And while this is less of a formal 
result, we recently were informed by a stakeholder at The Nature Conservancy
that our predictions were between 90 and 99 percent accurate to their own 
field measurements.

# Slide 16

So then we move into the second part of our framework, where we use spaceborne
remote sensing data to start extrapolating beyond the reach of our LiDAR data.
And so we retrain our models using the predictions from our LiDAR models, and
then produce new models that can predict biomass for the entire state. We can 
then use those models to generate predictions for every year from 1990 to 2019, 
which is the span we have FIA data to compare against, and then assess those 
using the same multiple-scale protocol as the LiDAR maps.

# Slide 17

And so we're then able to use these maps to start to get a sense of how the
amount of carbon stored in forests across the state changes over time.
We can start to fill in those measurement gaps so we can start managing our
lands to maximize carbon storage across the state.

# Slide 18

And one very cool thing about having these fine-scale timeseries is that 
they let us detect all sorts of changes in forest structure that we can't pick 
up based on FIA measurements alone. For instance, this is a quick map showing 
our biomass predictions around an area that was cut down in 2018 -- more green
here means more biomass. And you can see that our models, between 2017 and 2019,
pick up pretty strongly on that harvest even while the surrounding area doesn't
change. We can get a much better sense of forest management activities than
we would be able to using just the FIA plot data.

# Slide 19

And so moving on to the other place we've applied this same framework,
we're also engaged in trying to 
prioritize the state's reforestation efforts -- areas to protect and maybe even
plant to promote more carbon storage.

# Slide 20

Now, there are a lot of smart and defensible opinions on how the state should
go about prioritizing reforestation. Without going too far into those, one
possible method would be to prioritize conserving areas that are already
reforesting on their own -- abandoned fields and young forests that will develop
into productive forests without needing any planting or help from the state.

So what we've done is applied that same framework of starting with individual 
point-in-time snapshots, based on LiDAR data, then moving to statewide predictions
based upon spaceborne remote sensing.

The challenge here though is that the FIA only measures interior forest lands.
There's not a great independent data set that we can use to validate our 
models of young forest against.

# Slide 21

So what we decided to do was to use one of the
leading land cover classification products, LCMAP, to try and identify vegetated
areas across the state, and remove non-vegetated lands from our maps.

We then combined that with our LiDAR data to identify 
areas that were between 1 and 5 meters tall, which is in line with what the NLCD
land cover product calls "shrubland". Whether we think of this as being 
"shrubland" or "young forest" or 
"areas that seem to have short vegetation based on remote sensing products", 
these are areas that are promising candidates for reforestation. 
And in total, we find that the areas we have LiDAR for are about 2.5% shrubland.

Now, something to highlight here is that this surface is not a machine learning
output. We're able to classify all of the area covered by LiDAR without needing
to fit any models. Which means that we're able to skip the step where we
fit models to our airborne LiDAR data, and instead move right along to 
extrapolating beyond these areas using spaceborne remote sensing data.

# Slide 22

And so just like before we fit an ensemble model, this time swapping out the
support vector machine for a neural network. 

# Slide 23

We then go ahead and predict the
areas covered by LiDAR for the years we have LiDAR data, in order to assess our
model accuracy.

And what we find with this approach is that, compared to our LiDAR surface, our
models are pretty good at telling shrubland apart from non-shrubland, with an 
AUC of 0.89.

# Slide 24

We can then go ahead and use the same exact models to extrapolate our predictions
to the rest of the state -- this here is the prediction surface for 2019.

Now, one problem with this surface is that, 
because shrubland is so rare across the state,
a low level of false positives translates into a large absolute number of false
positive predictions.

# Slide 25

So what we've done is identified a number of classification thresholds that we
use to actually classify our predictions, raising the bar required to consider
a pixel "shrubland" to try and reduce false positives overall. So now we're 
able to offer multiple products, tuned to your tolerance for false positive
predictions.

And so, that's a quick overview of some of the ways 
New York is trying to use AI and ML to fill in some of its measurement gaps 
when it comes to forest structure, to try and manage for a net zero carbon
future. 

# Slide 26

But I wanted to wrap up my talk today by connecting these projects 
directly to the theme of our session on fairness and FAIRness in machine 
learning.

By now, I hope it's clear that we're engaged in a lot of data-intensive projects
to try and help New York fill in measurement gaps and manage its forest lands.
And one of our largest challenges, as a data-intensive project team, is 
figuring out how we can integrate new data streams into our workflows. 
We maintain a central data mart for our research group, where we store a set of 
standardized, pre-processed and validated data for use in our models.
Every time we add a product to that set we also add a commitment for our small 
team to track updates to that data 
product and to reprocess and upload the data on our end should anything change.
There's a world in which all our data sources are publishing sufficient 
human- and machine-readable metadata that we can automate that burden away, but
right now it's a serious obstacle to incorporating new products into our workflow.

In the other direction, we are also a producer of data products, and we're 
still in the process of figuring out how we can release our data in a format that
best serves all our stakeholders. Open data and open science often prioritize those
who know how to code and to decode scientific jargon to the exclusion of 
everyone else, and that's not a good place to be if we're producing data 
products that are being used in management decisions that impact the lives
of people across the state. Most people can't grab data from an API and have
no idea how to read XML. So, we need to find a way that we can make this 
information actually useful and usable to everyone who needs it or might be
impacted by it.

# Slide 27

And so on that note, I want to thank you so much for your time! If you want,
you can find me at mike mahoney two one eight on most platforms, or at my 
website m m two one eight dot dev.

Thank you again!
