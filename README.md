# Log Puzzle
For this exercise, you'll use Python code to solve two puzzles. This exercise uses the `urllib.request` module. Add your code to the `logpuzzle.py` file.

An image of an animal has been broken into many narrow vertical stripe images. The stripe images are on the internet somewhere, each with its own URL. The URLs are hidden in a web server log file. Your mission is to find the urls and download all image stripes to re-create the original image.

The slice URLs are hidden inside apache log files (the open source [Apache](http://httpd.apache.org/) web server is the most widely used server on the Internet). Each log file is from some server, and the desired slice URLs are hidden within the logs. The log file encodes what server it comes from as follows:

- the log file `animal_code.google.com` is from the `code.google.com` server (formally, we'll say that the server name is whatever follows the first underscore).
- The `animal_code.google.com` log file contains the data for the "animal" puzzle image. Although the data in the log files has the syntax of a real Apache web server, the data beyond what's needed for the puzzle is randomized data from a real log file.

Here is what a single line from the log file looks like (this really is what Apache log files look like):
```
10.254.254.28 - - [06/Aug/2007:00:14:08 -0700] "GET /foo/talks/ HTTP/1.1"
200 5910 "-" "Mozilla/5.0 (X11; U; Linux i686 (x86_64); en-US; rv:1.8.1.4) Gecko/20070515 Firefox/2.0.0.4"
```
The first few numbers are the IP address of the requesting browser. The most interesting part is the "GET _path_ HTTP" showing the path of a web request received by the server. The path itself never contains spaces and is separated from the GET and HTTP by spaces (regex suggestion: `\S` (upper case S) matches any non-space char). Find the lines in the log file where the string "puzzle" appears inside the path, ignoring the many other lines in the log.

## Part A - Log File To URLs
Complete the `read_urls(filename)` function that extracts the puzzle URLs from inside a log file. Find all the "puzzle" path URLs in the log file. Combine the path from each URL with the server name from the filename to form a full URL (e.g., `http://www.example.com/path/puzzle/from/inside/file`). Screen out URLs that appear more than once. The `read_urls()` function should return the list of full URLs, sorted in alphabetical order and without duplicates. Taking the URLs in alphabetical order will yield the image slices in the correct left-to-right order to re-create the original animal image. In the simplest case, `main()` should just print the URLs, one per line.

```console
% python logpuzzle.py animal_code.google.com
http://code.google.com/something/puzzle-animal-baaa.jpg
http://code.google.com/something/puzzle-animal-baab.jpg
...
```

## Part B - Download Images Puzzle
Complete the `download_images()` function which takes a sorted list of URLs and a destination directory. Download the image from each URL into the given directory, creating the directory first if necessary (see the `os` module for creating a directory, and `urllib.request.urlretrieve()` for downloading a URL). Name the local image files with a simple scheme like "img0", "img1", "img2", and so on. You may wish to print a helpful "Retrieving..." status output line while downloading each image since it can be slow and its nice to have some indication that the program is working. Each image is a little vertical slice from the original. How do you put the slices together to re-create the original? It can be solved nicely with a little HTML (knowledge of HTML is not required).

The `download_images()` function should also create an `index.html` file in the directory with an `<img>` tag to show each local image file. The `img` tags should all be on one line together without separation. In this way, the browser displays all the slices together seamlessly. You do not need knowledge of HTML to do this; just create an index.html file that looks like this:

```html
<html>
<body>
<img src="/edu/python/exercises/img0"><img src="/edu/python/exercises/img1"><img src="/edu/python/exercises/img2">...
</body>
</html>
```

Of course your index file won't actually use the `/edu/python/exercises/` src path for images; it will use the directory path specified in the `--todir` command line option. Here's what it should look like once you can download the animal puzzle.

```
% python logpuzzle.py --todir animaldir animal_code.google.com
% ls animaldir
img0  img1  img2  img3  img4  img5  img6  img7  img8  img9  index.html
```

When it's all working, opening the index.html in a browser should reveal the original animal image. What is the animal in the image?

<img src="https://i.ytimg.com/vi/SxvyKqkCXy0/maxresdefault.jpg" width="300" />

## Part C - Image Slice Descrambling
The second puzzle involves an image of a very famous place, but depends on some custom sorting. For the first puzzle, the URLs can be sorted alphabetically to order the images correctly. In the sort, the whole URL is used. However, for this part, we'll say that if the URL ends in the pattern `bar-_letters-moreletters_.jpg` (e.g., `http://example.com/foo/puzzle/bar-abab-baaa.jpg`), then the URL should be represented by the **last** word (e.g. "baaa") when sorting. So, sorting a list of URLs each ending with the `letters-moreletters`.jpg pattern should order the URLs by whichever word is in the `moreletters` position.

Extend your code to order such URLs properly, and then you should be able to decode the second `place_code.google.com` puzzle which shows a famous place. What place does it show?

## Guidance
- We are looking for readable, maintainable, performant code. Appropriate comments are expected
- Your solution must pass a PEP8 (flake8) check with 0 errors reported
- You must use at least one regular expression in your solution somewhere
- Your solution must use the `with` statement (Python context manager)
- You must use the `if __name__ == '__main__'` Python idiom, and define a `main()` function
- It is *OKAY* to add your own helper functions to achieve a solution
- You will need an active Internet connection in order to download the images
- If your socket connection times out within `urllib`, try again

## Submitting your work
To submit your solution for grading, you will need to create a github [Pull Request (PR)](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests).  Refer to the `PR Workflow` article in your course content for details.

## Credits
Creative Commons (CC) Attribution: The images used in this puzzle were made available by their owners under the [Creative Commons Attribution 2.5](http://creativecommons.org/licenses/by/2.5/) license, which generously encourages remixes of the content such as this one. The animal image is from the Flickr user `zappowbang` and the place image is from the Flickr user `booleansplit`.

