---
title: "Getting Started with R on Windows"
datePublished: Fri Jul 01 2016 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrmewpp703ntfws1eo2h3qpd
slug: getting-started-with-r-on-windows-ba6b8daacfae
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410491717/hAwelJ-uu.jpeg
tags: data-science, r, data-visualisation-1

---


This tutorial is meant to be a bare-minimum installation guide and mini “getting started” guide. If you will be using R extensively, there are other tools that you will probably want to install and use. I’ll go over some of those at the end of this article.

![Photo by [David Goehring](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410490120/JXC7HEClr.html) under [CC BY 2.0](https://creativecommons.org/licenses/by/2.0/)](https://cdn-images-1.medium.com/max/2048/1*1YvE_CwmG_rJ5ZtYYQKJ_w.jpeg)*Photo by [David Goehring](https://www.flickr.com/photos/carbonnyc/) under [CC BY 2.0](https://creativecommons.org/licenses/by/2.0/)*

### What is R and why would I use it?

R is a programming language created for working with data and statistics. It is very useful for data analysis and data mining.

### Installing

R is mainly distributed through [The Comprehensive R Archive Network](https://cran.r-project.org/) — or, CRAN. Some of the first links you will see on the CRAN homepage are download links for Linux, Mac OS, and Windows. Since we are using Windows for the purposes of this tutorial, you can click **Download R for Windows**.

On this page, you have a few options. The **contrib** link contains packages for use with R. The **Rtools** link contains tools to build packages yourself. For now, we just want to install the base distribution of R, so click **base**.

The first link on this page should say **Download R [version] for Windows**. Go ahead and click that. As the download completes, take a look at the other information on this page. There are further instructions, a list of new features in the latest version, checksums for verifying your download, an FAQ, and some other information if you’re interested.

Once the download completes, go ahead and run the executable. Then…

1. Select your language (probably English if you’re reading this tutorial) and click **OK**

1. Click **Next >** through the welcome screen

1. Read the GNU license and click **Next > **again

1. Select an installation directory. The default is probably fine. When you’re ready click **Next >**

1. The official R documentation recommends installing the “native” version for your operating system. If you are using a 32-bit operating system, select **32-bit User installation** from the dropdown menu. If you are using a 64-bit operating system, choose **64-bit User installation**. If you’re not sure what version you’re running, press Win + Pause and look under the **System type**. When you are ready, click **Next >**

1. You will now be asked whether you want to customize the startup options. The defaults are fine, so just select **No (accept defaults)** and click **Next >**

1. Click **Next >** through the start menu folder options since we *do* want shortcuts in our start menu

1. Click **Next >** through the additional tasks options since the defaults are fine.

At this point R will start installing. Once it is done, click **Finish** to exit the installer.

### Using R

Congratulations! R is installed. Now what can we do with it? R is very powerful and there is so much you can do with it. However, today I’ll just show off a few features.

To start off, we’ll need some data. For all of these examples, I’ll use the following CSV data. Please copy and paste the data below into a new file called **students.csv** and save it in **This PC** &gt; **Documents**.

```
school,    grade, firstName,   lastName, attHrs, absHrs
Lincoln,       2, Elizabeth,     Brooks,    100,     12
Washington,    2,    Hailey,     Walker,    293,     34
Washington,    1,    Joseph,     Powell,    485,     56
Central,       2,      Anna,    Simmons,    484,      7
Lincoln,       3,     Isaac, Washington,    320,     13
Washington,    3,      Jack,   Thompson,    129,     25
Central,       3,    Daniel,       Bell,    345,     37
Lincoln,       1,      Anna,       Reed,     73,     48
Central,       1,     Caleb,     Barnes,    284,      0
```


The next step is to import this data into an R environment. Go ahead and start up the **RGui** by clicking **Start** &gt; **All apps** &gt; **R** &gt; **R x64 3.3.1** (your version may be different, but similar to mine).

In the **RGui**, you should see an **R Console** window. This is where you can type in different commands to control R. First, let’s import the students.csv data and save it to the **students** variable.

```
students = read.csv("students.csv")
```


If something went wrong, it was probably because R couldn’t find your **students.csv** file. By default, RGui looks in **This PC** &gt; **Documents**, but if you’ve saved your file somewhere else, you can switch to the correct directory by clicking **File &gt; Change dir…**

As long as everything worked correctly, you should be able to just type **students** and it should print our data to the console.

```
> students

      school grade      firstName        lastName attHrs absHrs
1    Lincoln     2      Elizabeth          Brooks    100     12
2 Washington     2         Hailey          Walker    293     34
3 Washington     1         Joseph          Powell    485     56
4    Central     2           Anna         Simmons    484      7
5    Lincoln     3          Isaac      Washington    320     13
6 Washington     3           Jack        Thompson    129     25
7    Central     3         Daniel            Bell    345     37
8    Lincoln     1           Anna            Reed     73     48
9    Central     1          Caleb          Barnes    284      0
```


The data type of the **students** variable is called a **data frame**. Data frames are like “tables” of data.

### Sorting

If, for example, you want the **students** data frame sorted by school, then grade, then lastName, then firstName, you can use the **order** function. The **order** function returns a permutation which rearranges arguments into an order. In this case, use it like so:

```
> students[ order(students[,1], students[,2], students[,4], students[,3]), ]

      school grade      firstName        lastName attHrs absHrs
9    Central     1          Caleb          Barnes    284      0
4    Central     2           Anna         Simmons    484      7
7    Central     3         Daniel            Bell    345     37
8    Lincoln     1           Anna            Reed     73     48
1    Lincoln     2      Elizabeth          Brooks    100     12
5    Lincoln     3          Isaac      Washington    320     13
3 Washington     1         Joseph          Powell    485     56
2 Washington     2         Hailey          Walker    293     34
6 Washington     3           Jack        Thompson    129     25
```


The order is given as arguments. To get school (column 1), grade (col 2), lastName (col 4), firstName (col 3), the arguments would be in the order 1, 2, 4, 3.

If you want descending order, you can make the argument negative, like so:

```
> students[ order(students[,1], -students[,2], students[,4], students[,3]), ]

      school grade      firstName        lastName attHrs absHrs
7    Central     3         Daniel            Bell    345     37
4    Central     2           Anna         Simmons    484      7
9    Central     1          Caleb          Barnes    284      0
5    Lincoln     3          Isaac      Washington    320     13
1    Lincoln     2      Elizabeth          Brooks    100     12
8    Lincoln     1           Anna            Reed     73     48
6 Washington     3           Jack        Thompson    129     25
2 Washington     2         Hailey          Walker    293     34
3 Washington     1         Joseph          Powell    485     56
```


Notice the minus sign in front of **students[,2]** and notice how the 2nd column (grade) is sorted in descending order now, after sorting by column 1 (school).

### Adding a computed column

Let’s say you wanted to add a new column to the **students** data frame that had the total hours the student could have attended school. We will call this column **memHrs** and it will be the sum of **attHrs** and **absHrs**. Use this command:

```
> students$memHrs <- students$attHrs + students$absHrs
```


If we look at **students** again, we see the added column!

```
> students

      school grade    firstName      lastName attHrs absHrs memHrs
1    Lincoln     2    Elizabeth        Brooks    100     12    112
2 Washington     2       Hailey        Walker    293     34    327
3 Washington     1       Joseph        Powell    485     56    541
4    Central     2         Anna       Simmons    484      7    491
5    Lincoln     3        Isaac    Washington    320     13    333
6 Washington     3         Jack      Thompson    129     25    154
7    Central     3       Daniel          Bell    345     37    382
8    Lincoln     1         Anna          Reed     73     48    121
9    Central     1        Caleb        Barnes    284      0    284
```


### Converting a column to a factor

Let’s assume that when the **students** data frame was being loaded, R considered the **grade** column to be an integer. It sure *looks* like an integer. They’re just whole numbers. But, in fact, it is merely a discrete number that describes a grade a student is in. We won’t be doing any sort of calculations on the grade, and besides, what if a student was in “kindergarten”? That can’t be represented as an integer. Grade should be a **factor** instead of an integer. To factor this column, use this command:

```
> students$grade <- factor(students$grade)
```


### Merging with a VLOOKUP-style process

In this scenario let’s introduce “school codes”. For the sake of example, each school has a school code associated with it. Here’s what the codes look like in CSV format. Save this data as **schoolCodes.csv**

```
name,       code
Central,     101
Lincoln,     102
Washington,  103
```


Now import the codes as a data frame.

```
> schoolCodes = read.csv("schoolCodes.csv")
> schoolCodes

        name code
1    Central  101
2    Lincoln  102
3 Washington  103
```


We’d like to add the **code** column from **schoolCodes** to the **students** data frame based on the **school** column (which contains the school’s name). Use this command:

```
> students$schoolCode <- merge(codes, students, by.x=”name”, by.y=”school”)$code
```


The **merge()** function by itself would return to the whole merged data frames. By adding **$code** to the end, we are only returning the **code** column. At the beginning of this command, you can see that we are setting the (new) **schoolCode** column to this value.

```
> students

      school  gra firstName lastName attHrs absHrs memHrs schoolCode
1    Lincoln    2 Elizabeth   Brooks    100     12    112        102
2 Washington    2    Hailey   Walker    293     34    327        103
3 Washington    1    Joseph   Powell    485     56    541        103
4    Central    2      Anna  Simmons    484      7    491        101
5    Lincoln    3     Isaac Washingt    320     13    333        102
6 Washington    3      Jack Thompson    129     25    154        103
7    Central    3    Daniel     Bell    345     37    382        101
8    Lincoln    1      Anna     Reed     73     48    121        102
9    Central    1     Caleb   Barnes    284      0    284        101
```


If you want to learn more about the R language, I’d suggest starting with the [Cookbook for R](http://www.cookbook-r.com/). Once you’re ready to start extending R, you’ll want to use some packages. Packages provide extended functionality in R that can be very useful for analyzing data more efficiently. Packages can be installed through the console or through RGui’s interface. You can find packages in [the CRAN package repository](https://cran.r-project.org/web/packages/).