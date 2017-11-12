# Notes from Reviewer

Well Done for optimizing the parameter setting to create a continues lines aligned with the lane lines.
Obviously, there are other solutions here. Please consider the following,
threshold = 50
min_line_len = 100
max_line_gap = 160

'max_line_gap' defined the maximum distance between segments that will be connected to a single line.
'min_line_len' defined the minimum length of a line that will be created.
Increasing these parameters will create smoother and longer lines

"threshold" defined the minimum number of intersections in a given grid cell that are required to choose a line.
Increasing this parameter, the filter will choose longer lines and ignore short lines.

http://docs.opencv.org/3.0-beta/doc/py_tutorials/py_imgproc/py_houghlines/py_houghlines.html