# HOMEWORK 3 #
In your third and the last homework assignment, which is built on your first and second homeworks, you will gain experience with manipulating Java bytecodes using Javassist, a specialized Java library that supports structural reflection by enabling programming to mutate the bytecode of an application at load-time using a classloader http://jboss-javassist.github.io/javassist. In addition, you will experiment with concurrency and parallelism using the Java Executor framework and thread pooling, and loading, modifying, and executing external Java applications from your launching application. YHW is built on the previous exercises where you will reuse the functionality of the Java parser and you learned and used. We refer to the codebase that you create as a result of this homework as a program and we refer to the Java code that your program processes as a Java application. The git repo for HW3 can be cloned using the command git clone drmark@bitbucket.org:CS_474_2017/cs474_2017_hw3.git.

A rationale for this homework is to create an efficient mutation testing tool, where mutation testing is an approach to inject faults in a software application to determine if existing tests detect these injected faults. You can read more about mutation testing in this {Wikipedia entry} (https://en.wikipedia.org/wiki/Mutation_testing). Mutation operators are specific functions that map the original application's code to a mutant (i.e., a modified bytecode) by injecting a fault into the original application's code in a predefined manner. For example, for the following code example, if( a > b ){...}, one possible mutant is `if( a <= b ){...}`, i.e., a Logical Operation (LOR) mutant. A comprehensible list of mutants is given in the papers on [Description of muJava’s Method-level Mutation Operators](https://cs.gmu.edu/~offutt/mujava/mutopsMethod.pdf) and the other paper on [Description of Class Mutation Mutation Operators for Java] (https://cs.gmu.edu/~offutt/mujava/mutopsClass.pdf). Your job is to implement a subset of these mutants, taking at least two mutation operators of your choice from each of the mutant subcategories. For the full implementation of all mutants described in these paper, you will receive an additional bonus of up to 5%.
 
A broad outline of the functionality of the Launcher, a program that you will implement as part of this homework, is the following. The inputs to Launcher include the instrumented bytecode of the open-source Java application that you have experimented with in the previous homework and a configuration file that specifies a range of input values for each of these inputs. For example, for an integer input the range could be [0-5], or if the input if a file, it could be specified as a set of the paths to the files. For bonus points, you can speficy regular expressions to [generate random input values] (https://github.com/icomefromthenet/ReverseRegex), for example. Your Launcher reads in the inputs and runs the input Java application in a thread, it collects its execution trace in a data structure (e.g., a list), and it determines all statements and operations from this trace that were executed in the Java application with the specific input. Next, your Launcher will determine which mutation operators are applicable to the executed statements and operations in the application. Within the Launcher you may define a mutation matrix with rows corresponding to the Java application's statements/operations and the columns corresponding to the chosen subset of mutation operators. Once it is determined which rows are of interest from the execution trace, a proper subset of cells for these rows is chosen to determine which mutation operators can be applied to these statements/operations that correspond to the chosen rows.
 
In the next step, armed with the information which code mutation to apply to what statements/operations, your Launcher will use a thread from the pool of threads to run a job, which uses Javassist to mutate a designated statement/operation. That is, each thread in the pool is assigned a mutation operator and a specific location in the bytecode to apply it to, using the information from the computed mutation matrix. The applications's instrumented bytecode is loaded in the thread, and once the bytecode is mutated with the Javassist functions, it is run using some class entry method, which is also specified in the configuration file. The trace for the mutated bytecode is also saved like the trace for the original, unmutated application's run and then the mutated application's trace is compared with the original trace to determine the differences in the states and control flows. 

As you can imagine, collecting traces and holding them in the RAM is quite expensive, since the traces occupy a lot of memory. Alternatively, saving them in files leads to a significant I/O overhead, which is quite expensive too. For additional bonus points, consider how you can optimize the execution. One idea is to reduce the size of the data in each trace element by using an integer identifier to reference a table that contains the list of all program statements, and use the hash code to reference the value or a set of values for the variables that are changed by this statement/operation. Other solutions are possible. Since improving the performance and the efficiency of applications is one of the most significant issues that many companies face, this is a great experience that you obtain as part of this homework. This completes the description of homework 3.
 
This homework differs from the previous two, since you are allowed to form groups. If you want to work alone, it is perfectly fine, however, you can decide to work in a group with up to two more of your classmates. Logistically, one of team members will create a private fork and will invite one or two of her classmates with the write access to your fork. You should be careful who you partner with - once you form a group and write and submit code, you cannot start dividing your work and claim you did most of the work. Your forkmates may turn out to be freeloaders and you will be screwed, but it is a part of your life experience. Be very careful and make sure that you trust your classmates before forming your group. Neither your TA not I can and will resolve your internal group conflicts, unless you can present convincing evidence that you did all the work alone. Your submission will include the names of all of your forkmates and you will receive the same grade for this homework. Working in a group will be an excellent opportunity for you to explore branching in git, merging, rebasing, and resolving semantic conflicts when merging your code changes. Don't pass on this opportunity!

If you submitted your previous homework, it means that you were already added as a member of CS_474_2017 team in Bitbucket. Separate repositories will be created for each of your homeworks and for the course project. You will find a corresponding entry for this homework. You will fork this repository and your fork will be private, no one else besides you, your forkmates, the TA and your course instructor will have access to your fork. Please remember to grant a read access to your repository to your TA and your instructor and write access to your forkmates. You can commit and push your code as many times as you want. Your code will not be visible and it should not be visible to other students except for your forkmate, of course. When you push your project, your instructor and the TA will see you code in your separate private fork. Making your fork public or inviting other students except for your forkmates to join your fork will result in losing your grade. For grading, only the latest push timed before the deadline will be considered. If you push after the deadline, your grade for the homework will be zero. For more information about using git and bitbucket specifically, please use this link as the starting point https://confluence.atlassian.com/bitbucket/bitbucket-cloud-documentation-home-221448814.html. For those of you who still struggle with Git, I keep recommending a book by Ryan Hodson on Ry's Git Tutorial. The other book called Pro Git is written by Scott Chacon and Ben Straub and published by Apress and it is freely available https://git-scm.com/book/en/v2/. There are multiple videos on youtube that go into details of Git organization and use.

As your TA specified, please follow this naming convention while submitting your work : "Firstname_Lastname_hw3", so that we can easily recognize your submission. Those who work in groups can use longer names: "Firstname1_Lastname1_Firstname2_Lastname2_Firstname3_Lastname3_hw3". I repeat, please make sure that you will give both your TA and me read access to your private forked repository.

As usual, I allow you to post questions and replies, statements, comments, discussion, etc. on Piazza either using your names or anonymously. Remember that you cannot share your code and your solutions beyond your forkmate group, but you can ask and advise others using Piazza on where resources and sample programs can be found on the internet, how to resolve dependencies and configuration issues, and how to design the logic of the algorithm, as usual. Yet, your implementation should be your own and you cannot share it beyond your forkmate group. Alternatively, you cannot copy and paste someone else's implementation and put your name on it. Your submissions will be checked for plagiarism. When posting question and answers on Piazza, please select the appropriate folder, i.e., hw3 to ensure that all discussion threads can be easily located.

Submission deadline: Sunday, November 12 at 5PM CST. Your submission will include your source code, the IntelliJ project, the SBT build configuration, the README.md file in the root directory that contains the description of your implementation, how to compile and run it using SBT, and what are the limitations of your implementation.

## Evaluation criteria ##
- the maximum grade for this homework is 10% + up to 3% bonus. Points are subtracted from this maximum grade: for example, saying that 2% is lost if some requirement is not completed means that the resulting grade will be 10%-2% => 8%;
- no comments or highly insufficient comments: up to 2% lost;
- no unit and integration tests: up to 5% lost;
- code does not compile or it crashes without completing the core functionality: up to 10% lost;
- the documentation is missing or insufficient to understand how to compile and run your program: up to 6% lost;
- only a subset of your tests works: up to 3% lost;
- the minimum grade for this homework cannot be less than zero.