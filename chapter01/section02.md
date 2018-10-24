
[*Chapter 1: An Introduction to Data Analysis*](./)
[*第一章：数据分析概论*](./)


# 1.2. Knowledge Domains of the Data Analyst
# 1.2. 数据分析师的知识领域


Data analysis is basically a discipline suitable to the study of problems that may occur in several fields of applications. Moreover, data analysis includes many tools and methodologies that require good knowledge of computing, mathematical, and statistical concepts.
数据分析基本上是一门适用于研究在几个应用领域中可能出现的问题的学科。此外，数据分析包括许多工具和方法，这些工具和方法需要对计算、数学和统计概念有很好的了解。
A good data analyst must be able to move and act in many different disciplinary areas. Many of these disciplines are the basis of the methods of data analysis, and proficiency in them is almost necessary. Knowledge of other disciplines is necessary depending on the area of application and study of the particular data analysis project you are about to undertake, and, more generally, sufficient experience in these areas can help you better understand the issues and the type of data needed.
一个优秀的数据分析人员必须能够在许多不同的学科领域采取行动和行动。其中许多学科是数据分析方法的基础，精通这些学科几乎是必要的。其他学科的知识是必要的，这取决于您将要从事的特定数据分析项目的应用和研究领域，更广泛地说，在这些领域的足够经验可以帮助您更好地理解所需的问题和数据类型。
Often, regarding major problems of data analysis, it is necessary to have an interdisciplinary team of experts who can contribute in the best possible way in their respective fields of competence. Regarding smaller problems, a good analyst must be able to recognize problems that arise during data analysis, inquire to determine which disciplines and skills are necessary to solve these problems, study these disciplines, and maybe even ask the most knowledgeable people in the sector. In short, the analyst must be able to know how to search not only for data, but also for information on how to treat that data.
通常，关于数据分析的主要问题，需要有一个跨学科的专家小组，他们可以在各自的能力领域以尽可能最好的方式作出贡献。对于较小的问题，一个好的分析师必须能够识别数据分析过程中出现的问题，询问需要哪些学科和技能来解决这些问题，学习这些学科，甚至可能询问行业中最有知识的人。简而言之，分析师必须知道如何不仅搜索数据，而且还要知道如何处理数据的信息。


## Computer Science
## 计算机科学

Knowledge of computer science is a basic requirement for any data analyst. In fact, only when you have good knowledge of and experience in computer science can you efficiently manage the necessary tools for data analysis. In fact, every step concerning data analysis involves using calculation software (such as IDL, MATLAB, etc.) and programming languages (such as C ++, Java, and Python).
计算机科学知识是任何数据分析师的基本要求。事实上，只有当你有良好的计算机科学知识和经验，你才能有效地管理必要的数据分析工具。事实上，数据分析的每一步都涉及到使用计算软件(如IDL、MATLAB等)和编程语言(如c++、Java、Python等)。
The large amount of data available today, thanks to information technology, requires specific skills in order to be managed as efficiently as possible. Indeed, data research and extraction require knowledge of these various formats. The data are structured and stored in files or database tables with particular formats. XML, JSON, or simply XLS or CSV files, are now the common formats for storing and collecting data, and many applications allow you to read and manage the data stored on them. When it comes to extracting data contained in a database, things are not so immediate, but you need to know the SQL query language or use software specially developed for the extraction of data from a given database.
由于信息技术的发展，今天所能获得的大量数据需要有特定的技能才能得到尽可能有效的管理。事实上，数据研究和提取需要了解这些不同的格式。数据是结构化的，并以特定格式存储在文件或数据库表中。XML、JSON或简单的XLS 或者CSV文件，现在是存储和收集数据的通用格式，许多应用程序允许您读取和管理存储在它们上的数据。当涉及到提取数据库中包含的数据时，事情并不是那么直接，但是您需要了解SQL查询语言，或者使用专门为从给定数据库中提取数据而开发的软件。
Moreover, for some specific types of data research, the data are not available in an explicit format, but are present in text files (documents and log files) or web pages, and shown as charts, measures, number of visitors, or HTML tables. This requires specific technical expertise for the parsing and the eventual extraction of these data (called web scraping).
此外，对于某些特定类型的数据研究，数据不是以显式格式提供，而是存在于文本文件(文档和日志文件)或网页中，并以图表、度量、访问者数量或HTML表的形式显示。这需要专门的技术知识来解析和最终提取这些数据(称为Web抓取)。
So, knowledge of information technology is necessary to know how to use the various tools made available by contemporary computer science, such as applications and programming languages. These tools, in turn, are needed to perform data analysis and data visualization.
因此，信息技术知识对于了解如何使用当代计算机科学提供的各种工具是必要的，例如应用程序和编程语言。这些工具依次用于执行数据分析和数据可视化。
The purpose of this book is to provide all the necessary knowledge, as far as possible, regarding the development of methodologies for data analysis. The book uses the Python programming language and specialized libraries that provide a decisive contribution to the performance of all the steps constituting data analysis, from data research to data mining, to publishing the results of the predictive model.
这本书的目的是提供所有必要的知识，尽可能，关于发展方法的数据分析。本书使用Python编程语言和专门的库，它们对构成数据分析的所有步骤的性能做出决定性贡献，从数据研究到数据挖掘，再到发布预测模型的结果。


## Mathematics and Statistics
## 数学和统计

As you will see throughout the book, data analysis requires a lot of complex math during the treatment and processing of data. You need to be competent in all of this, at least to understand what you are doing. Some familiarity with the main statistical
正如你在整本书中所看到的，在数据的处理和处理过程中，数据分析需要很多复杂的数学。你需要胜任所有这一切，至少要理解你在做什么。熟悉主要统计数据
concepts is also necessary because all the methods that are applied in the analysis and interpretation of data are based on these concepts. Just as you can say that computer science gives you the tools for data analysis, so you can say that the statistics provide the concepts that form the basis of data analysis.
概念也是必要的，因为所有用于分析和解释数据的方法都是基于这些概念的。正如你可以说计算机科学给你提供了数据分析的工具，所以你可以说统计数据提供了构成数据分析基础的概念。
This discipline provides many tools to the analyst, and a good knowledge of how to best use them requires years of experience. Among the most commonly used statistical techniques in data analysis are
这一规则为分析师提供了许多工具，而对如何最好地使用这些工具的良好知识需要多年的经验。在数据分析中最常用的统计技术包括：
* Bayesian methods
* 贝叶斯方法
* Regression
* 回归
* Clustering
* 聚集
Having to deal with these cases, you’ll discover how mathematics and statistics are closely related. Thanks to the special Python libraries covered in this book, you will be able to manage and handle them.
必须处理这些情况，您将发现数学和统计学是如何密切相关的。由于本书中介绍了特殊的Python库，您将能够管理和处理它们。


## Machine Learning and Artificial Intelligence
## 机器学习和人工智能

One of the most advanced tools that falls in the data analysis camp is machine learning. In fact, despite the data visualization and techniques such as clustering and regression, which should help you find information about the dataset, during this phase of research, you may often prefer to use special procedures that are highly specialized in searching patterns within the dataset.
属于数据分析阵营的最先进的工具之一是机器学习。事实上，尽管数据可视化和技术，如聚类和回归(这将帮助您找到有关数据集的信息)，在这一阶段的研究中，您可能更喜欢使用在数据集中搜索模式的高度专门化的特殊过程。
Machine learning is a discipline that uses a whole series of procedures and algorithms that analyze the data in order to recognize patterns, clusters, or trends and then extracts useful information for data analysis in an automated way.
机器学习是一门学科，它使用一系列的过程和算法来分析数据，以便识别模式、集群或趋势，然后以自动化的方式提取有用的信息进行数据分析。
This discipline is increasingly becoming a fundamental tool of data analysis, and thus knowledge of it, at least in general, is of fundamental importance to the data analyst.
这门学科正日益成为数据分析的基本工具，因此，至少在一般情况下，对数据分析人员来说，对它的了解是至关重要的。

## Professional Fields of Application
## 专业应用领域

Another very important point is the domain of competence of the data
另一个非常重要的问题是数据的权限范围。
(its source—biology, physics, finance, materials testing, statistics on population, etc.). In fact, although analysts have had specialized preparation in the field of statistics, they must also be able to document the source of the data, with the aim of perceiving and better understanding the mechanisms that generated the data. In fact, the data are not simple strings or numbers; they are the expression, or rather the measure, of any parameter observed. Thus, better understanding where the data came from can improve their interpretation. Often, however, this is too costly for data analysts, even ones with the best intentions, and so it is good practice to find consultants or key figures to whom you can pose the right questions.
(来源:生物、物理、金融、材料测试、人口统计等)。事实上，虽然分析人员在统计领域有专门的准备工作，但他们也必须能够记录数据的来源，以便了解和更好地了解产生数据的机制。事实上，数据不是简单的字符串或数字;它们是任何事物的表达方式，或者更确切地说是对观测参数衡量标准 .因此，更好地了解这些数据的来源可以改进它们的解释。然而，对于数据分析人员来说，这往往代价太大，即使他们的意图是最好的，因此，找出你可以提出正确问题的顾问或关键人物是很好的做法。
