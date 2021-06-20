 # ln, link
 
 It is useful for maintaining multiple copies of a file in many places at once without using up storage for the __copies; instead, a link points__ to the original copy.
There are two types of links; hard links and symbolic links.  How a link __points__ to a file is one of the differences between a __hard__ and __symbolic__ link.

We will use for the moment only symblic links (using the __-s__ option).\
Symbolic links behave as a shortcut to a file.
 
## Syntax: 
__ln [options] source_file [link_name]__ 

__-s__    Create a symbolic link.\
__-f__ If the proposed link (link_name) already exists, then unlink it so that the link may occur.\
__-n__ If the link_name is a symbolic link, do not follow it. This is most useful with the -f option, to replace a symlink which may point to a directory.

//to do
exemple cu fiecare parametru\
-n ce face exact? daca link_name deja exista si foloses ln fara _-n_ ce se va intampla?
