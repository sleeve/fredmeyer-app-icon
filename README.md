# Fred Meyer App Icon

After recently interviewing at Kroger Digital for a Quality Engineer role I had some free time over the Thanksgiving holiday for a side project. I had already started to investigate the Fred Meyer iOS app and compiled a list of bugs and improvements to reference in the interview but I wanted to take a deeper dive into one of them.                                                            really loved meeting the team and wanted to 

I've also wanted to secretly become a designer over the years, so exercises like these help me understand more about the craft. I think the outcome of this project does a lot of things correctly but I'm sure I overlooked or mis-interpreted something along the line. Would love to hear feedback from anyone if they notice anything out of the ordinary with my work.

I believe that attention to detail is a critical trait that every great qe should have. Here is an example of the level of detail that I'm capable of showing. it also shows that I am self-motivated and self-directed and can learn things on my own (and eventually share them) without someone guiding me. its ok if you don't really see the need for the project, I totally understand that some people will see it that way and that it may even be the majority of people. I enjoy the little details in projects and believe that there is a right and wrong way to do things. That is pretty much what software is, 1s and 0s, true or false, right and wrong. micro-bugs.

learning and trying understand more about color spaces, color profiles, wide-gamut, rgb, srgb, display p3 and hex codes. sketch, imagemagick, identify, pixelmator, preview, colorsync utility, xscope, xcode, ios simulator, pngcrush, kaleidoscope, asset catalog tinkerer, acextract, apple configurator 2, iterm2, visual studio code, tower, firefox, git, github, markdown, pixelmator pro, optipng

usually not best practice to have a logo extend to the edge of the app icon image. can't find it in the current ios app logo guidelines though, used to be in there though. usually better to a little room around the edges for the logo to breathe. I get what the kroger designers where thinking though with the new recent bold brand refresh. the new kroger logo is amazing, really love how its simple looking but still has a lot of character. The app icon with the new 'K' is also super solid. its simple, friendly yet interesting and bold at the same time -- super cool. the other banner stores/logos/icons didn't get the same attention though, which is totally understandable when you have 20 other brands to upgrade. 

one thing that they did seem to bring over is the app icon design theme of using only the first letter/character of the banner store name/logo and stretching it slightly beyond the bounds of the app icon edges creating an purposeful artistic clipping effect to the character. this works amazing for the kroger icon but not so much for all the others. for all the other banner stores is looks more like a bug than an artistic choice. an example is on the top of kroger icon, the top part of the "K" character isn't clipped, you see the top most part of the fancy K 

one big assumption that I'm making is that I'm pretty sure the current red color used in the logo is wrong and doesn't match the brand guidelines. I could be wrong on this but I can't find any overwhelming evidence that disproves my theory. I'm mostly relying on how the logo is displayed on the fred meyer website and also from the svg on fredmeyer.com which has the exact rgb hex code within it. The brand colors may have been recently refreshed but if so, they decided to go with a less vibrant color and land on a de-saturated duller red.

https://www.thekrogerco.com/about-kroger/our-brand/
https://www.underconsideration.com/brandnew/archives/new_logo_and_identity_for_kroger_by_ddb.php

I would love to sit down and chat with all of the designers who worked on the new app icons and understand what direction they wanted to take and what their process is like. Hopefully I could share some tips that helped me but also hear the problems they are trying to solve.

Wanted to investigate using Display P3 versions of the icon but through a lot of trial and error I'm pretty sure the difference between the versions would be negligible. I was getting confused switching between both sRGB and Display P3 color spaces/profiles. I was keeping the same 8-bit red color hex code (#ED1C24) for both the sRGB and Display P3 image export but just assigning them their respective color profiles. This was producing a correct sRGB image but by just assigning the Display P3 color profile to the Display P3 image it was making the red color way more saturated and vibrant than it really should be. At first I thought this is just what Display P3 images should look like as they're supposed to be able to display more colors that we're not used to seeing in the sRGB world. The problem is that the red sRGB color value is already within the Display P3 gamut so we shouldn't really be seeing any new crazy vibrant wider colors.

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
Fred-Meyer-01.svg (https://www.brandeps.com/logo-download/F/Fred-Meyer-01.zip) -- same color as fm.com svg
https://encycolorpedia.com/companies/us/fred-meyer
https://encycolorpedia.com/ed1c24

#d8262c \
Fred-Meyer-01.eps (https://www.brandeps.com/logo-download/F/Fred-Meyer-01.zip) -- looks more muted, doesn't match the other icons included within the zip.

#eb1c25 \
Fred-Meyer-01.jpg (https://www.brandeps.com/logo-download/F/Fred-Meyer-01.zip) -- looks closer to fm.com svg colors.






# Starting Fresh (for everyone 😉)
After the main investigation we have enough data to create a fresh new app icon set with all the fixes applied along with some bonus fixes.

1. creating a new sketch document with a 1024x1024px white square background layer.
2. open the svg we got from fm.com also within sketch, copy the "F" vector path within it and paste it in our new document over the white background
3. select the "F" path and make sure its in front of the background layer and then update the fill color to #ed1c24 that was the color used in the original svg.
4. stretch the "F" path to take up the full height of the 1024px background, be sure to hold shift while stretching it to maintain aspect ratio. sketch should help us out a bit by snapping to the top and bottom edges when we're close.
5. Once the "F" path is tall enough at 1024px we need to make sure its also centered within the background, drag the "F" path around a bit and sketch should display some centering guidelines and snap to them when we're close to them.
6. So that should be enough work to be able to now export a .png from it but lets make one more minor improvement that has to do with pixel hinting. https://dcurt.is/pixel-fitting I find it best to only use pixel hinting on the straight right angle parts of logos like these and to mostly leave the more round or angular sides of shapes/characters untouched. Moving to many vector points around will change the way the logo looks so its best to keep it to a minimum and not go too crazy trying to get every single line to fit perfectly within all pixels. you can only do so much with pixel hinting. we really only needed to fix two edges/lines of the "F" path in the 1024px version.
7. We can now export the sketch document as a .png, 8-bit color, sRGB color profile
8. Checking the exported image it has an alpha channel included within it which we don't need. pngcrush seems to remove this one with -brute. maybe be a good idea to find specific imagemagick command to remove all unneeded png chunks. what is a needed chunk though?


Seeing some bugs or potential issues with how xcode is bundling icons? why does it create AppIcon76x76@2x~ipad.png in the .app bundle when I only have icon assets within my asset bundle? what are packed assets when looking at the assets.car file within asset catalog tinkerer? why is fred_meyer-ios_icon-srgb-180px-60pt_Normal@2x.png and fred_meyer-ios_icon-srgb-180px-60pt_Normal@3x.png the same after extracting from asset catalog tinkerer? could that be from the packed asset? is it just a bug with asset catalog tinkerer? why do files get the "_Normal" suffix added to them after using asset catalog tinkerer? why do the extracted images have more metadata included with them compared to when I added them to the project? Don't have time to dig into this anymore... I just know that the png image assets that you include within an xcode project will not necessarily match what you can extract from the Assets.car file with third party tools. Not sure if its the way Xcode compiles them or if it is the third party tools.

optipng is amazing and even better than pngcrush. `optipng -v -o7 -strip all -keep image.png`
the file size savings is great on the larger 1024px app store icon but not quite as much on the smaller images. still a slight improvement though over pngcrush as we're able to strip out a bit more of the metadata with optipng.

optipng (5 test files) = 14,627 bytes (29 KB on disk)
pngcrush (5 test files) = 21,082 bytes (33 KB on disk)
