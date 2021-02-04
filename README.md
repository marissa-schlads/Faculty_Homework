Statistics 433: Faculty Table
================
Marissa Schladweiler

``` r
# The UW-Madison Faculty information used for this project can be found at the URL below
# The rvest R package allows you to easily scrape data from webpages (similar to BeautifulSoup in Python)
library(rvest)

# read_html automatically inputs the information from this URL into R
faculty_page = read_html("https://guide.wisc.edu/faculty/")
```

To extract only the faculty information we want (name, position,
department, degree info), it was necessary to find the appropriate tag
that encodes this desired information. This was done by using the
Developer Tools in the Chrome web browser settings where it was found
that each new person was encoded in paragraph nodes represented by
“p”

``` r
# html_nodes() finds all nodes that match the tag (in our case, "p") and puts each of those elements in a list
faculty_nodes = faculty_page %>% html_nodes("p")

# Now we have a list of each faculty members information in a list (each row is a new person)
# However, <br> is separating the name from the position and so on, so we use xml_find_all and xml_add_sibling to find all breaks and replace them with ";" for easier separation later
xml_find_all(faculty_nodes, ".//br") %>% xml_add_sibling("p", ";")

# Now we use html_text() to get the raw text of faculty information (now separated by semicolons), and remove non-faculty information (first two rows and last row)
faculty_nodes = html_text(faculty_nodes)[c(-1, -2, -3991)]
```

``` r
# This loops through each person in the list, extracts their name, position, department, and degree info and 
# binds them to data frame where each person is a new row
library(stringr)
faculty_data = data.frame()
for(i in 1:length(faculty_nodes)){
  cols = str_split(faculty_nodes[i], ";")[[1]][1:4]
  faculty_data = rbind(faculty_data, cols, stringsAsFactors=FALSE)
}
```

``` r
# This changes the column names and displays the data frame
library(kableExtra)
colnames(faculty_data) = c("Name", "Position", "Department", "Degree Info")
kbl(faculty_data[1:20,])
```

<table>

<thead>

<tr>

<th style="text-align:left;">

Name

</th>

<th style="text-align:left;">

Position

</th>

<th style="text-align:left;">

Department

</th>

<th style="text-align:left;">

Degree Info

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

AARLI,LISA

</td>

<td style="text-align:left;">

Lecturer

</td>

<td style="text-align:left;">

Counseling Psychology

</td>

<td style="text-align:left;">

EDM 2000 Univ of Wisconsin-Madison

</td>

</tr>

<tr>

<td style="text-align:left;">

ABBOTT,DANIEL E

</td>

<td style="text-align:left;">

Assoc Professor (CHS)

</td>

<td style="text-align:left;">

Surgery

</td>

<td style="text-align:left;">

MD 2016 University of Washington

</td>

</tr>

<tr>

<td style="text-align:left;">

ABBOTT,DAVID H.

</td>

<td style="text-align:left;">

Professor

</td>

<td style="text-align:left;">

Obstetrics & Gynecology

</td>

<td style="text-align:left;">

PHD 1979 University of Edinburgh

</td>

</tr>

<tr>

<td style="text-align:left;">

ABBOTT,NICHOLAS

</td>

<td style="text-align:left;">

Professor

</td>

<td style="text-align:left;">

Chemical & Biological Engr

</td>

<td style="text-align:left;">

PHD 1991 Massachusetts Inst Of Tech

</td>

</tr>

<tr>

<td style="text-align:left;">

ABD-ELSAYED,ALAA A

</td>

<td style="text-align:left;">

Asst Professor (CHS)

</td>

<td style="text-align:left;">

Anesthesiology

</td>

<td style="text-align:left;">

MD 2000

</td>

</tr>

<tr>

<td style="text-align:left;">

ABDUALLAH,FAISAL

</td>

<td style="text-align:left;">

Associate Professor

</td>

<td style="text-align:left;">

Art

</td>

<td style="text-align:left;">

PHD 2012 Royal College of Art

</td>

</tr>

<tr>

<td style="text-align:left;">

ABRAHAM,OLUFUNMILOLA

</td>

<td style="text-align:left;">

Assistant Professor

</td>

<td style="text-align:left;">

Pharmacy

</td>

<td style="text-align:left;">

PHD 2013 Univ of Wisconsin-Madison

</td>

</tr>

<tr>

<td style="text-align:left;">

ABRAMS,SAMANTHA

</td>

<td style="text-align:left;">

Assoc Lecturer

</td>

<td style="text-align:left;">

Information School

</td>

<td style="text-align:left;">

MA 2017 Univ of Wisconsin-Madison

</td>

</tr>

<tr>

<td style="text-align:left;">

ABRAMSON,LYN

</td>

<td style="text-align:left;">

Professor

</td>

<td style="text-align:left;">

Psychology

</td>

<td style="text-align:left;">

PHD 1978 University of Pennsylvania

</td>

</tr>

<tr>

<td style="text-align:left;">

ACKER,LINDSAY

</td>

<td style="text-align:left;">

Lecturer

</td>

<td style="text-align:left;">

Accting & Info Sys

</td>

<td style="text-align:left;">

MACC 2005 Univ of Wisconsin-Madison

</td>

</tr>

<tr>

<td style="text-align:left;">

ACKERMAN,STEVEN

</td>

<td style="text-align:left;">

Professor

</td>

<td style="text-align:left;">

Atmospheric & Oceanic Sciences

</td>

<td style="text-align:left;">

PHD 1987 Colorado State University

</td>

</tr>

<tr>

<td style="text-align:left;">

ADAMCZYK,PETER GABRIEL

</td>

<td style="text-align:left;">

Assistant Professor

</td>

<td style="text-align:left;">

Mechanical Engineering

</td>

<td style="text-align:left;">

PHD 2008 Univ of Michigan at Ann Arbor

</td>

</tr>

<tr>

<td style="text-align:left;">

ADAMS,AERON

</td>

<td style="text-align:left;">

Clinical Asst Prof

</td>

<td style="text-align:left;">

Nursing

</td>

<td style="text-align:left;">

DNP 2017 Univ of Wisconsin-Madison

</td>

</tr>

<tr>

<td style="text-align:left;">

ADAMS,MEGAN

</td>

<td style="text-align:left;">

Asst Faculty Assoc

</td>

<td style="text-align:left;">

Information School

</td>

<td style="text-align:left;">

MA University of South Florida

</td>

</tr>

<tr>

<td style="text-align:left;">

ADAMS,TERESA

</td>

<td style="text-align:left;">

Professor

</td>

<td style="text-align:left;">

Civil & Environmental Engr

</td>

<td style="text-align:left;">

PHD 1989 Carnegie-Mellon University

</td>

</tr>

<tr>

<td style="text-align:left;">

ADDINGTON,REBECCA LYNN

</td>

<td style="text-align:left;">

Senior Lecturer

</td>

<td style="text-align:left;">

Psychology

</td>

<td style="text-align:left;">

PHD 1998 Univ of Wisconsin-Madison

</td>

</tr>

<tr>

<td style="text-align:left;">

ADDO,FENABA R.

</td>

<td style="text-align:left;">

Assistant Professor

</td>

<td style="text-align:left;">

School Of Human Ecology

</td>

<td style="text-align:left;">

PHD 2012 Ithaca College

</td>

</tr>

<tr>

<td style="text-align:left;">

ADELL,SANDRA

</td>

<td style="text-align:left;">

Professor

</td>

<td style="text-align:left;">

Afro-American Studies

</td>

<td style="text-align:left;">

PHD 1989 Univ of Wisconsin-Madison

</td>

</tr>

<tr>

<td style="text-align:left;">

ADHAMI,VAQAR

</td>

<td style="text-align:left;">

Senior Scientist

</td>

<td style="text-align:left;">

Dermatology

</td>

<td style="text-align:left;">

PHD 1998 Jamia Hamdard

</td>

</tr>

<tr>

<td style="text-align:left;">

ADLER,HANS

</td>

<td style="text-align:left;">

Professor

</td>

<td style="text-align:left;">

German, Nordic & Slavic

</td>

<td style="text-align:left;">

PHD 1978 Ruhr Universitat Bochum

</td>

</tr>

</tbody>

</table>
