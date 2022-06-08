# ~sitful's 3DP gun noob guide

So, you're interested in building guns with a 3D printer. I started looking into this about two years ago, and have since built a handful of guns, and I'm collating my resources here to pass along what I've learned. Printing guns is surprisingly fun and easy, even without any 3DP experience. 

## Printer

First things first, you'll need a printer. I only have experience with one, but it's the most commonly recommended -- the [Ender 3 Pro](https://www.amazon.com/s?k=ender+3+pro). This is an open source design that is fully modular -- pretty much every piece of it can be upgraded or modified, and there's a huge 3rd party accessory market. Most 3D gun guides will assume you have an Ender 3.

If you want to gamble, you can get a [heavily discounted](https://www.comgrow.com/products/used-ender-3-ender-3-pro?variant=40070400770091) used Ender that may or may not be missing parts.


## Building & Calibration

It'll take a little tinkering before you can jump into printing. You'll need to assemble your printer, and do calibration before you end up wasting any filament on frames.

I watched [this video](https://www.youtube.com/watch?v=me8Qrwh907Q) when I built my printer. It's worth taking your time and making sure everything is put together well -- if anything is a little off, it can have big consequences and be difficult to nail down the reason. 3D printers are very tempermental and require a lot of tweaking, so try to get ahead of that as much as you can at this stage.

3D models usually come as `.stl` files, which you will need to convert to `.gcode` files -- these are basically machine instructions for the printer, including stuff like temperature. The software you use to convert it is called [Cura](https://ultimaker.com/software/ultimaker-cura) -- there's other software for this, but Cura is the only one I've used.

You can find good settings for Cura [here](https://threadreaderapp.com/thread/1452771276603330561.html). Or, you can just [download](https://urbits3.ams3.digitaloceanspaces.com/sitful-hatred/2022.6.08..22.03.57-sitful-cura.curaprofile) my Cura profile settings.

A skill that you will need to master is [leveling your bed](https://www.youtube.com/watch?v=RJC7N4Vb9AY) with a piece of paper. You'll find yourself doing this a lot -- it's natural for your printer's bed to become slightly off balance as you use it. It's not hard to fix and dramatically improves print quality.

Once your printer is assembled, you'll need to calibrate. The simplest way is to print a [calibration cube](https://all3dp.com/2/3d-printer-calibration-cube-the-best-models-how-to-use-them/) and measure the results with [calipers](https://www.amazon.com/dp/B00B5XJW7I/) to calculate the cause of any issues. You can follow [this guide](https://all3dp.com/2/how-to-calibrate-a-3d-printer-simply-explained/).

There are also a handful of popular models used to tell how well tuned your printer is. 

Here's an example of a simple [level benchmark](https://www.thingiverse.com/thing:2187071). You can print this and look at the result to get an idea of how 'off' your bed level is. If the layers seem kind of squiggly and not smoothly adhered to each other, your nozzle is probably too high. Don't let it scrape the bed, though. 

Here is a popular model named [Benchy](https://www.thingiverse.com/thing:763622), which will test how well your printer can handle difficult features like overhangs. If you can print a good Benchy, your printer is good to go for whatever else. 

## Printing material

If you're printing without major upgrades you'll probably be doing everything with PLA+, which is a tougher version of the bog-standard material PLA. There are lots of vendors, but my favorite (and one I generally see good things about) is [Polymaker](https://us.polymaker.com/products/polyterra-pla-plus?variant=39574351380537). They are cheap, ship super fast, and have excellent quality.

If you want to get into printing with fancy materials, you'll probably need to upgrade the control board and flash it with [Marlin](https://marlinfw.org/), which is a custom firmware that you can modify and reflash. This allows you to do things like increase the maximum print temperature, which is necessary for some materials. You can follow [this guide](https://www.youtube.com/watch?v=RbbzsJBpEhc&list=WL&index=4).

## Gun resources

Here's the fun stuff! If you can print a decent Benchy, you can probably print a decent gun frame or receiver. Here I'll just list useful resources for printing guns -- if you're already calibrated and your Cura is good to go, you can try downloading a frame, slicing it and printing.

[Ctrl-pew](https://ctrlpew.com/) is basically *the* web resource for printing guns and the knowledge related to it. 

However, if you have a Twitter, find and follow some of the 3D printer guys to keep a closer eye on what's happening in the scene. Here are a few of the big names: [CtrlPew](https://twitter.com/CtrlPew2), [Vinh Nguyen](https://twitter.com/nguyenkvvn), [Ivan](https://twitter.com/NaviGoBoom), [chairmanwon](https://twitter.com/chairmanwon), [3DPGeneral](https://twitter.com/3DPrintGeneral), [RKSpookware](https://twitter.com/RKSpookware)

[The Gatalog](https://thegatalog.com/) contains links to most of the publicly available gun models. 

[Hoffman Tactical](https://hoffmantactical.com/) is another guy who releases models, so far just AR-related. 

Most gun models are available on [LBRY](https://lbry.com/), which has a web app called [Odysee](https://odysee.com/), but you can't search files (as opposed to media) unless you download their app.

When you build a gun, you're usually only printing part of it. You'll still need to buy gun parts (though crucially, they are not legally controlled in the same way). So if you want to build an AR, you can print the [lower receiver](https://odysee.com/@TheGatalog-PrintableFramesReceivers:9/UBAR-Rev1:e) and [upper receiver](https://odysee.com/@TheGatalog-PrintableFramesReceivers:9/BidensBane_3DAR15_Upper_Receiver:e), and grip, stock, etc, but at the very least you'll need to buy things like the FCG, BCG, charging handle, barrel, etc. Usually with an AR in particular, you just print the lower and buy everything else, since the lower is the part that is legally a gun. As time goes on, expect more parts to become printable.

In any case, there are a lot of vendors that sell gun parts specifically for 3DP guns. If you build a glock, there are a couple of parts that are not included with a slide assembly or a parts kit, specifically the rails and locking block. You can buy these from [Aves Rails](https://www.avesrails.com/) or [RK Spookware](https://rkspookware.com/glock-fmda-ddxx-2/). A few other vendors for miscellaneous models are [3D Defense](https://www.3ddefense.net/), [DB Firearms](https://www.db-firearms.com/store), and [JSD Supply](https://jsdsupply.com/3d-printed-gun-frames/). These parts are usually meant for specific 3D gun models that are specified in the listings. 


## Optional upgrades

If you upgrade anything for print quality then usually getting a glass bed, upgraded bed springs and a ptfe bowden tube are the first things on the menu. If you want to print exotic (hard, high-temp) materials like [carbon fiber reinforced nylon](https://coexllc.com/product/black-coexnylex-carbon-filled-nylon-2-85/), you'll need to upgrade the extruder, hotend, nozzle, and control board. I also always put a light mist of hair spray on my print bed right before I print to help adhesion.

Note that there are other equivalent pieces of kit available, these are just suggestions. None of these are necessary, though I'd strongly recommend at least upgrading the springs & Bowden tube.


#### General upgrades

[Springs & Bowden tube](https://www.amazon.com/dp/B081DN6RM2/) -- $20

[Printer enclosure](https://www.amazon.com/dp/B0995VTBLS) -- $65

[Direct drive extruder](https://www.amazon.com/dp/B09WR8CJZD) -- $110

[Auto bed leveler](https://www.amazon.com/dp/B0979F7RWN) -- $40 (requires upgraded control board)

[32bit control board](https://www.amazon.com/dp/B088W6517D) -- $80

#### Exotic material upgrades

[Upgraded hotend](https://www.sliceengineering.com/collections/hotends/products/mosquito-for-creality) -- $130

[Steel extruder gears](https://www.amazon.com/dp/B08BYPRY8T) -- $11

(note that these are for the stock extruder on Ender3, I'm not sure how they work with direct drive extruders)

[Steel print nozzles](https://www.amazon.com/dp/B09533GXB8) -- $10

#### Fun upgrades

If you have a spare RPi 3 or above, you can install [Octoprint](https://octoprint.org/) on it, hook it up to your Ender's USB port, and monitor and control your prints remotely, including with an auxiliary webcam. I haven't tried this but it's on my list, there are even mobile apps for interfacing with it. 