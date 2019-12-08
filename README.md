# Fred Meyer App Icon

After recently interviewing at Kroger Digital (better known as Fred Meyer in the northwestern region of the US) for a Quality Engineer role I had some free time over the Thanksgiving holiday for a side project. I had already started to investigate the Fred Meyer iOS app and compiled a list of bugs and improvements to reference in the interview but I didn't get a chance to go over everything that I uncovered. I really enjoyed meeting the team and wanted to continue my investigation to show just how eager I was to join the team. One thing that was mentioned during the interview was that a previous QE there had handed over a packet of 75 bugs/improvements during their interview. The interviewers seemed pretty impressed by this and it's an amazing total but not that unrealistic for any app from a non-tech company.

One bug that I had previously come across was this weird app icon distortion issue when backgrounding the app. I was curious about why it was happening and I had a hunch that I could fix it with a little more investigation. Just taking a quick glance at the icon I started to notice some other potential issues with it. I'm a competitive person on occasion and I had this historical number of "75 bugs" floating around in my head and it seemed like a fun target. I thought it would be a little hilarious to see how close I could get to 75 bugs/improvements before even launching app. It sounds like a ridiculous target but I got very close and may have actually exceeded that number...

## Why Waste Your Time On This?

I believe that attention to detail is a critical trait that every great QE should have. This project hopefully is a good example of the level of detail that I'm capable of. It's definitely not a high priority issue but I don't work there yet so I have no other tasks assigned to me. I also have secretly wanted to become a designer over the years. Exercises like these help me understand more about the craft and give me more experience with various design tools. I totally understand most people who see the final icon comparisons will see no value and ask "why would you waste your time on this"? That's totally fine and understandable but I'm a person who appreciates these big little details and am passionate about getting them right. That's really the essence of software; 1's and 0's, true or false, right and wrong. In the end though it's just a slightly humorous and educational project for **_me_**.

## The First Bug

Here is the first issue that I noticed with the app icon.

![the_first_bug](https://github.com/sleeve/fredmeyer-app-icon/blob/master/screenshots/the_first_bug.gif)

You should be able to see the thick grey banding on the bottom of the icon during the backgrounding animation. Why would it be doing that? There are plenty of other similar looking app icons with a logo on top of a white background but none of them seemed to exhibit the same banding behavior. So there must be something wrong with the app or icon asset. When playing around with the main Kroger app I noticed the same thing happening with their blue Kroger app icon. Is this a problem on all 30+ Kroger apps? Not quite but it is a problem on about 50% of them. More on that later.

![kroger_banding](https://github.com/sleeve/fredmeyer-app-icon/blob/master/screenshots/kroger_banding.png)

So how do we investigate this further? We need a way to access the actual icon image assets but without having access to any of the source files. We used to be able to just download an app .ipa bundle file within iTunes but in recent versions of macOS, iTunes has been removed. There is a way to install an old version but it doesn't really work for what we're trying to do. I used a sort of hacky method with Apple Configurator 2 to download a backup .ipa of the Fred Meyer app from the App Store. This gave me access to the whole Fred Meyer.app container and all the files within it. There are some loose image assets within it but most are locked away in the main compiled Asset Catalog file (Assets.car). There are some handy open source tools which allow you to extract the image assets within the .car file. I used Asset Catalog Tinkerer and extracted everything that I could. I was able to track down all of the raw .png app icon assets this way. I started going through them trying to find the thick grey banding but they all looked pretty normal in Preview.app.

![asset_catalog_example](https://github.com/sleeve/fredmeyer-app-icon/blob/master/screenshots/asset_catalog_example.png)

I then gathered all of the app icon assets and added them to an iOS test app that I created. I was able to reproduce the issue with my test app so now I knew the issue was with the icon assets and had nothing to do with the apps.

![fred_test](https://github.com/sleeve/fredmeyer-app-icon/blob/master/screenshots/fred_test.gif)

If we change the Preview.app canvas to white (#FFFFFF) we start to see something fishy with the icon. The right and bottom sides of the icon seem to have a darker grey border. It should theoretically be all white (#FFFFFF) and match the white canvas color.

![asset_catalog_example_white](https://github.com/sleeve/fredmeyer-app-icon/blob/master/screenshots/asset_catalog_example_white.png)

Zooming in on it a bit more we can see that there is indeed a 1-pixel grey border on the right and bottom sides of the icons.

![grey_pixel_border](https://github.com/sleeve/fredmeyer-app-icon/blob/master/screenshots/grey_pixel_border.png)

So now I knew that this 1-pixel grey line was somehow responsible for the backgrounding animation issue. I was still confused on how but once I slowed down the animation within the iOS simulator I started to see why. During the animation iOS seems to grap the whole bottom row of pixels of an app icon and stretch it down to make it feel a bit more immersive when launching or closing an app. Look at the blue Kroger icon screenshot above and you can see the stretching effect on the right leg of the "K". This feels like more of an iOS bug to me though, it's an animation hack but it works fine on 99% of the icons. Most modern app icons don't have content that touches the edge of the icon. The majority of icons consist of small logos or glyphs centered on a mostly solid color background. You wouldn't really ever notice this animation bug if you didn't accidentally have a row of dark pixels on your mostly white icon. You can even see the behavior mid-animation on the Apple Maps icon.

![apple_maps](https://github.com/sleeve/fredmeyer-app-icon/blob/master/screenshots/apple_maps.png)

I went over all of the individual sizes for the icon assets and saw that the grey border wasn't on every single size but it was on 75% of them. We could have just gone in with an image editor and changed the grey pixels to white but by this time I had seen enough of the icon to spot a few other things I could improve. Fixing all of them would be pretty impossible to pull of by just using an image editor. I decided that if I wanted it done correctly I would have to basically re-create the icon from scratch.

## Tweaking The Brand Refresh

Before we dive in and start making the new icon lets go over some of the other fixes that I want to include in it. Kroger recently did a [brand refresh](https://www.thekrogerco.com/about-kroger/our-brand/) which included a new updated Kroger logo and marketing campaign. I really like the look of the new logo and the way they applied it to the main Kroger app icon. Aside from the animation banding issue mentioned earlier I think it's an amazing icon. It's simple but bold and has some character to it. It uses just the first letter of the logo and it's positioned off center on the bottom-left to showcase the more expressive loop in the upper-right. Some of the edges of the character are slightly clipped off but it's very minor.

![kroger_icon](https://github.com/sleeve/fredmeyer-app-icon/blob/master/screenshots/kroger_icon.png)

I was a little surprised to see some branding experts [mostly-negative opinions](https://www.underconsideration.com/brandnew/archives/new_logo_and_identity_for_kroger_by_ddb.php) on the brand refresh but I think the implementation into the icon is perfect. The other 15 or so banner stores (Fred Meyer, Ralphs, Fry's, etc.) didn't quite get the same amount of love and attention for the refresh. None of the banner store logos were updated but they still decided to bring over the app icon refresh for all their various apps. I don't quite agree with this choice but I can understand the idea behind it. A lot of the other logos use very odd outdated wordmarks so just showing giant slightly-clipped versions of the first character for all those apps feels very lazy. My sympathies to the designers though, I can totally imagine the time pressure they must have been under to crank out 15+ new perfect bold app icons at once. Not easy an easy job.

![banner_icons](https://github.com/sleeve/fredmeyer-app-icon/blob/master/screenshots/banner_icons.png)

Luckily the Fred Meyer logo is much more modern looking than the other banner logos. Here's what the app icon used to look like before the brand refresh and how it looks like in the current version.

![fred_meyer_pre_refresh](https://github.com/sleeve/fredmeyer-app-icon/blob/master/screenshots/fred_meyer_pre_refresh.png)
![fred_meyer_icon](https://github.com/sleeve/fredmeyer-app-icon/blob/master/screenshots/fred_meyer_icon.png)

The new icon is fine and definitely bigger and bolder but I thought the previous version was already pretty good (except the registered trademark symbol). My initial impression of a giant red "F" didn't make me feel super comfortable and inspire me with confidence. It reminded me more of receiving a failing grade on a school project. You hate to see it.

![failing_grade](https://github.com/sleeve/fredmeyer-app-icon/blob/master/screenshots/failing_grade.jpg)

It's mostly fine though and it's easier to re-create from scratch so we're keeping the current icon design as our baseline. One improvement that I'm adding is to not clip off any part of the red "F" by extending it beyond the icons edges. In the current version the bottom and the top are just slightly clipped. It's usually not the best practice to have a logo touch the edges of the icon. It helps to leave a little space around the edges for the logo to breathe. This technique works for the new Kroger blue icon, just not so much on all the other banner store wordmarks though.

## What Color Is This?

Another thing that was bothering me was the new icon seemed to use a bit duller and de-saturated red color. I wanted to find out what the actual Fred Meyer red brand color was but I couldn't find any official brand guidelines. In order to narrow in on an official red brand color I decided to run color tests with [xScope](https://xscopeapp.com/) on all the Fred Meyer app icons and various logos within the app and around the web. The new app icons consistently use hex code color #D9272E but I was able to find multiple references around the web that pointed to #ED1C24 being the actual brand color.

![xscope](https://github.com/sleeve/fredmeyer-app-icon/blob/master/screenshots/xscope.png)

1. There's actually a Fred Meyer [vector logo](https://www.fredmeyer.com/content/v2/binary/image/fredmeyer_svg_logo-desktop-1556238715657.svg) on the website that has a fill color of #ED1C24.
2. The [favicon](https://www.fredmeyer.com/favicon.ico) on the website uses #ED1C24.
3. The [vector logo](https://upload.wikimedia.org/wikipedia/commons/7/79/Fred_Meyer_logo.svg) on Wikipedia uses #ED1C24.
4. The [vector logo](https://www.brandeps.com/logo-download/F/Fred-Meyer-01.zip) on brandeps.com uses #ED1C24.
5. There's a Fred Meyer company page on [encycolorpedia.com](https://encycolorpedia.com/companies/us/fred-meyer) that references #ED1C24 as the brand color.

This seemed like enough evidence that #ED1C24 was the real red brand color. We'll use this as the red color when re-creating the icons. I could be totally wrong in assuming this is the correct red as the brand colors may have been recently updated. I just think it's a bit more saturated and looks better on screen so we're going with it.

More recent iOS devices have wide color gamut displays so I wanted to learn more about digital color spaces and see if we could create better looking icons using the Display P3 color space. Xcode now supports using two different icon assets with for both sRGB and Display P3 color profiles. I started off by keeping the sRGB color hex code #ED1C24 and just assigning the Display P3 color profile to it. It looked amazing on screen next to the current app icon but it was way too vibrant and saturated. It almost looked like neon red and it just didn't feel quite right.

![display_p3](https://github.com/sleeve/fredmeyer-app-icon/blob/master/screenshots/display_p3.png)

I read a lot of articles and blog posts about working with different color spaces and how to use color profiles correctly. Instead of assigning a Display P3 color profile to the same sRGB color code I should have converted the colors into Display P3. After a lot of trial and error I came to the conclusion that we wouldn't see any real benefit by adding Display P3 versions of the app icons. The red brand color is already within the sRGB color space so by converting it to Display P3 our eyes would still see the same red color. Display P3 color profiles work best on photographs and more complex artwork. You just can't really see any difference on flat app icons that use a minimal color pallet.

## Pixel Fitted

We pretty much have all we need now to start to re-create the icon. We were able to get a copy of the vector logo so we can copy the "F" from it and have it be as sharp as possible in all resolutions. We have the red brand color that we're going to fill the "F" with. That's about all we need to get started but there is one more improvement that I'd like to add to all the icons. It's called [pixel-fitting](https://dcurt.is/pixel-fitting) (shout out to Dustin Curtis for the amazing blog post!). If we zoom into one of the current app icons we can see the effects of not pixel-fitting. Take a look at the horizontal edges within the "F" path. Notice how they're blurry and have a line of more pink-ish pixels where it transitions from red to white.

![blurry_pixels](https://github.com/sleeve/fredmeyer-app-icon/blob/master/screenshots/blurry_pixels.png)

We'll have to manually adjust these horizontal vector points for all 13 icon sizes that we need for Xcode. This is a lot more work to remove most of the fractional pixels but it's going to produce super sharp icons. The smaller icon sizes will see the most benefit. We're technically modifying the actual shape of the "F" but it shouldn't be noticeable if we do correctly.

## Starting Fresh (For Everyone) 😉

I think we have our plan laid out and have everything that we need now to re-create the icons. We're going to be using [Sketch](https://www.sketch.com/) as our vector editor. Here are the steps we're following starting with the largest 1024x1024px size:

1. Create a 1024x1024 square and fill it with white #FFFFFF, this is our icon background.
2. Copy the "F" vector path from the .svg we found on the website and paste it on top of the white background.
3. Stretch and fit the "F" path (while maintaining its aspect ratio) to be as tall as the background and centered within it.
4. Select the "F" path and fill it with the red brand color #ED1C24.
5. In order to maintain the correct angles in the "F" path we need to draw 6 vector guidelines around the horizontal points that we'll be moving before we start pixel-fitting. These will help us place the vector points correctly and not throw off the geometry of the "F" path.

![guidelines](https://github.com/sleeve/fredmeyer-app-icon/blob/master/screenshots/guidelines.png)

6. Now we can zoom way into those vector points along the horizontal axis to see the fractional pixels that we want to turn into full pixels. Drag the horizontal vector point to the closest pixel edge on the y-axis and then center it within the guideline we created on the x-axis. Do this for all 6 horizontal points.

![pixel_fitting](https://github.com/sleeve/fredmeyer-app-icon/blob/master/screenshots/pixel_fitting.gif)

7. Hide the guideline layers so only the background and "F" path are visible.
8. Export the icon at 1x size as a .png with an sRGB color profile and uncheck both "Interlace PNG" and "Save for web".

Most designers would just stop here after creating the 1024px version of the icon and then use it to create scaled down versions of the icon. There's even an official template within Sketch that automates this super quickly. We could use this method as it's a lot faster to create all the sizes of icons but we would miss out on all the pixel-fitting improvements if we automate it. This automation also seems to be the cause of our original grey banding animation bug. The designer created the original 1024px version and kicked off the automation that down-scaled and created all the smaller icons sizes. I'm not sure what tool they used but somewhere during that procedure the grey border started to appear on most of the icon sizes.

Since we're choosing to include pixel-fitting we'll need to repeat these steps for the 12 other required iOS app icon sizes. This was by far the most time consuming step of the whole project. The results turned out great though.

--------------------------------------------------------------------------



Checking the exported image it has an alpha channel included within it which we don't need. pngcrush seems to remove this one with -brute. maybe be a good idea to find specific imagemagick command to remove all unneeded png chunks. what is a needed chunk though?


Seeing some bugs or potential issues with how xcode is bundling icons? why does it create AppIcon76x76@2x~ipad.png in the .app bundle when I only have icon assets within my asset bundle? what are packed assets when looking at the assets.car file within asset catalog tinkerer? why is fred_meyer-ios_icon-srgb-180px-60pt_Normal@2x.png and fred_meyer-ios_icon-srgb-180px-60pt_Normal@3x.png the same after extracting from asset catalog tinkerer? could that be from the packed asset? is it just a bug with asset catalog tinkerer? why do files get the "_Normal" suffix added to them after using asset catalog tinkerer? why do the extracted images have more metadata included with them compared to when I added them to the project? Don't have time to dig into this anymore... I just know that the png image assets that you include within an xcode project will not necessarily match what you can extract from the Assets.car file with third party tools. Not sure if its the way Xcode compiles them or if it is the third party tools.

optipng is amazing and even better than pngcrush. `optipng -v -o7 -strip all -keep image.png`
the file size savings is great on the larger 1024px app store icon but not quite as much on the smaller images. still a slight improvement though over pngcrush as we're able to strip out a bit more of the metadata with optipng.

optipng (5 test files) = 14,627 bytes (29 KB on disk)
pngcrush (5 test files) = 21,082 bytes (33 KB on disk)

learning and trying understand more about color spaces, color profiles, wide-gamut, rgb, srgb, display p3 and hex codes. sketch, imagemagick, identify, pixelmator, preview, colorsync utility, xscope, xcode, ios simulator, pngcrush, kaleidoscope, asset catalog tinkerer, acextract, apple configurator 2, iterm2, visual studio code, tower, firefox, git, github, markdown, pixelmator pro, optipng, licecap, quicktime player, homebrew,

I think the outcome of this project does a lot of things correctly but I'm sure I overlooked or mis-interpreted something along the line. Would love to hear feedback from anyone if they notice anything out of the ordinary with my work.

![good_job](https://media.giphy.com/media/8xgqLTTgWqHWU/giphy.gif)


**App Logo/Icon colors:**

#d7282e \
ipa-iTunesArtwork.png - looks like theres some artifacting on the edges within the red, possibly saved as a jpeg at one point? white color measures as #feffff in pixelmator and sketch but not preview? extra metadata (exif/jfif?) included within it?

#d9272e \
app-AppIconFredMeyer76x76@2x~ipad.png -- looks pretty good, no grey outline \
app-AppIconFredMeyer60x60@2x.png -- grey single-pixel outline on right and bottom \
fred_meyer_ffe_logo_Normal.png -- remove the registered trademark and trademark symbols \
fred_meyer_ffe_logo_Normal@2x.png -- remove the registered trademark and trademark symbols \
fred_meyer_ffe_logo_Normal@3x.png -- remove the registered trademark and trademark symbols \
FredMeyerStores_Logo_RGB_Normal.png -- giant image could be smaller, remove the registered trademark symbol \
FredMeyerStores_Logo_RGB_Normal@2x.png -- giant image could be smaller, remove the registered trademark symbol \
FredMeyerStores_Logo_RGB_Normal@3x.png -- giant image could be smaller, remove the registered trademark symbol \
iOS-app-store-icon-1024x1024_Normal.png -- looks good, better than the actual iTunesArtwork.png in the .ipa \
ZZZZPackedAsset-1.1_Normal.png -- not quite sure what the packed asset is, maybe a pdf with multiple icon sizes? black outline around the whole collection, not sure if its a side effect of trying to extract the assets. most of the icons in this are good but the smallest one has a grey outline on the right and bottom. \
ZZZZPackedAsset-2.1_Normal@2x.png -- grey outline on right and bottom of the largest icon. \
ZZZZPackedAsset-3.1_Normal@3x.png -- grey outline on right and bottom of icon. \
fred-meyer-app-icon-20x20_Normal.png -- grey outline on right and bottom of icon. \
fred-meyer-app-icon-20x20_Normal@2x.png -- looks good \
fred-meyer-app-icon-20x20_Normal@3x.png -- grey outline on right and bottom of icon. \
fred-meyer-app-icon-29x29_Normal.png -- looks good \
fred-meyer-app-icon-29x29_Normal@2x.png -- looks good \
fred-meyer-app-icon-29x29_Normal@3x.png -- looks good \
fred-meyer-app-icon-40x40_Normal.png -- looks good \
fred-meyer-app-icon-40x40_Normal@2x.png -- grey outline on right and bottom of icon. \
fred-meyer-app-icon-40x40_Normal@3x.png -- grey outline on right and bottom of icon. probably incorrectly using this icon within the AppIcon asset catalog as the icon for 60pt 120px @2x iPhone app icon since the correct 60px@2x icon is the incorrect size. \
fred-meyer-app-icon-60x60_Normal@2x.png -- grey outline on right and bottom of icon. 180px instead of 120px displays an error when added to asset catalog. probably using 40pt@3x icon incorrectly instead to skate around the error/warning. \
fred-meyer-app-icon-60x60_Normal@3x.png -- grey outline on right and bottom of icon. \
fred-meyer-app-icon-76x76_Normal.png -- looks good \
fred-meyer-app-icon-76x76_Normal@2x.png -- looks good \
fred-meyer-app-icon-83.5x83.5_Normal@2x.png -- grey outline on right and bottom of icon.

#fffffe -- these probably were meant to be completely white, they all just slightly off though. I'd also remove the super-small unrecognizable registered trademark symbol. we shouldn't need to include it in any logo. fred meyer and kroger are established brands and logos no need to clutter up the legibility of the logo with it. \
80x80_White_FredMeyer_Normal.png \
80x80_White_FredMeyer_Normal@2x.png \
80x80_White_FredMeyer_Normal@3x.png

#ffffff -- color looks good on these, but I would also remove the registered trademark and trademark symbols, just makes it harder to visually scan and read. Also they are kind of large, not sure where they're being displayed but depending on that it might be possible to reduce the size of them. \
fred_meyer_ffe_logo_lockup_KO_Normal.png \
fred_meyer_ffe_logo_lockup_KO_Normal@2x.png \
fred_meyer_ffe_logo_lockup_KO_Normal@3x.png

**Vector Logo colors**

sRGB: #ed1c24 (237, 28, 36) Display P3: #DA3832 (218, 56, 50) \
https://www.fredmeyer.com/content/v2/binary/image/fredmeyer_svg_logo-desktop-1556238715657.svg -- looks way better than the current app icon/logo color, way more vibrant. measured in preview and within firefox and had same results. even lists `path fill="#ed1c24"` within .svg. \
fredmeyer.com favicon -- also measures as this within firefox tabs.
Fred-Meyer-01.png (https://www.brandeps.com/logo-download/F/Fred-Meyer-01.zip) -- same color as fm.com svg \
Fred-Meyer-01.svg (https://www.brandeps.com/logo-download/F/Fred-Meyer-01.zip) -- same color as fm.com svg \
Fred_Meyer_Logo (https://upload.wikimedia.org/wikipedia/commons/7/79/Fred_Meyer_logo.svg)
https://encycolorpedia.com/companies/us/fred-meyer
https://encycolorpedia.com/ed1c24

#d8262c \
Fred-Meyer-01.eps (https://www.brandeps.com/logo-download/F/Fred-Meyer-01.zip) -- looks more muted, doesn't match the other icons included within the zip.

#eb1c25 \
Fred-Meyer-01.jpg (https://www.brandeps.com/logo-download/F/Fred-Meyer-01.zip) -- looks closer to fm.com svg colors.
