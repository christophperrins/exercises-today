# Python with Jenkins

# Overview
Python can be useful because it helps us run a script of instructions. This can be incredibly useful when using CI servers like jenkins because it allows us to code very quickly in python due to the easy nature of its programming, and then have the CI server run these commands.

## Tutorial and Exercises
### Updating Readmes with python
#### Setting up SSH keys with github
If you have got SSH keys set up with github, you can move to the next section.

Keys are a way to authenticate that you are who you say you are. During the TCP handshake, it verifies that the identity matches that of previously recorded information.

##### Creating an SSH key
In a bash terminal write the command **ssh-keygen** and press enter through the following steps:
```
user@machine:~$ ssh-keygen

Generating public/private rsa key pair.
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ubuntu/.ssh/id_rsa.
Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:TzqTzNS9Dp8oa/LEaQi6C2AhryAk3L1pDGppjH3I2WI ubuntu@ubuntu
The key's randomart image is:
+---[RSA 3072]----+
|                 |
|. . .            |
|o+ o .           |
|*+++o o  . .     |
|=BE +=  S o .    |
|Bo +.. * *   .   |
|o .   . % o .    |
| . .  .ooo = .   |
|  o.   +oo. +    |
+----[SHA256]-----+
```

Read your public key using:
```
$ cat ~/.ssh/id_rsa.pub

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCzfjA70YaacZdgYig0urm8S2EIaii9zq2n7F4UyoZPgp+VrBd6YDcFpMUaEdSDR5DCMY3HVOJxjf2zDfHCd/VqGOjI0+yaniz+EzEAbPfEnlRFRGnzIaCJlHt0L27bLO9eIEvy0uerjdPUxU6ROM9AdepHbTtMxZUtfbkqTtdsY+JvLAzVz47zj9EJuezJx3vyHKq8XXfhXqG2LeHlV+u7c7PtodJQW+bYeKka/fz7LyM2dMBCna8XaVkEncJqH3cJVI7FumCXN9pb4IYqh9B4CCqhRAC89OOrSFjO17xdD3OrGd2DmwNpHgrXtMbNTAvgpsECI8q7PxQ5/co/G4tJ bob@work-laptop
```
This is your public ssh key, copy this as we will need it later

For more information go to the Linux section of [courseware](https://courseware.qa-portal.co.uk/learning/course/linux/module/linux--openssh).

##### Adding SSH key to github
Go to settings on github:

![settings](https://i.imgur.com/5Eu7V4B.png)

Under settings go to the tab, go to **SSH keys and GPG keys** and then select **new SSH key**. From the public key printed out earlier (the piece of text which beings with ssh-rsa...) copy all of it (including the ssh-rsa, and the user@machine). Paste that into the key section and give it a title which you wil recognise

#### Grabbing the total coverage of tests
Go to https://github.com/christophperrins/ims-demo and fork it to your own repository.

Once it is in your own repository click **clone or download** button and make sure it is using **clone with SSH**, the text should read something like the following (with your own github username):
> git@github.com:USERNAME/ims-demo.git

Copy this link and in a bash terminal run:
```bash
$ git clone git@github.com:USERNAME/ims-demo.git
```

If you commit a change and push it back up, it will no longer ask for your username and password because it is going to verify your credentials using your ssh keys.



## Task 1a: Calculate the test coverage with python
First generate the test analysis
``` bash
cd ims-demo
mvn test
```

After this you will find a target file, navigate to the following folder:
```bash
cd target/site/jacoco
```

Inside this location you will see a jacoco.csv file, open it with an application that can open spreadsheets. You will see that in columns 4 and 5, the headings INSTRUCTION_MISSED and INSTRUCTION_COVERED. Total coverage is going to be the sum of INSTRUCTION_COVERED divided by (sum of INSTRUCTION_COVERED + sum  of INSTRUCTION_MISSED)

Your task is to read in the csv file with python, and calculate the total percentage.

1) Create a folder called **automation** and put it at the root directory of the folder (the same place as src and pom.xml are.)
2) Inside there add a python file called calculate_test_coverage.py


<details><summary>DO NOT CHEAT. Click here to compare answers</summary>

```python
#!/usr/bin/python

import csv

def calculated_test_coverage():
    with open(file="target/site/jacoco/jacoco.csv", mode="r") as file:
        csv_reader = csv.reader(file, delimiter=',') # makes csv easier to handle
        next(csv_reader) # skip titles on first line
        covered = 0
        missed = 0
        for lines in csv_reader: 
            missed += int(lines[3])
            covered += int(lines[4])
        return str(int(covered/(missed+covered)*100))+"%"

print(calculated_test_coverage())

```
</details>

## Task 1b: Add the test coverage to the file

<details><summary>DO NOT CHEAT. Click here to compare answers</summary>

Its late but this is a solution
```python
#!/usr/bin/python

import csv

def calculated_test_coverage():
    with open(file="target/site/jacoco/jacoco.csv", mode="r") as file:
        csv_reader = csv.reader(file, delimiter=',') # makes csv easier to handle
        next(csv_reader) # skip titles on first line
        covered = 0
        missed = 0
        for lines in csv_reader: 
            missed += int(lines[3])
            covered += int(lines[4])
        return str(int(covered/(missed+covered)*100))+"%"

def add_test_coverage_to_readme():
    file_input = open(file="README.md", mode="r")
    readme_text = file_input.read()
    file_input.close()

    file_output = open(file="README.md", mode="w")
    file_output.write("Coverage: " + calculated_test_coverage() +"\n")
    file_output.write(readme_text)
    file_output.close()
    
def update_test_coverage_in_readme():
    file_input = open(file="README.md", mode="r")
    text = ""
    while True:
        line = file_input.readline()
        if line == "":
            break
   
        start_line = line.split(":")
        if start_line[0] == "Coverage":
            text += "Coverage: "+calculated_test_coverage()+"\n"
        else:
            text += line
    file_input.close()
    file_output = open(file="README.md", mode="w")
    file_output.write(text)
    file_output.close

def add_or_update():
    file_input = open(file="README.md", mode="r")
    while True:
        line = file_input.readline()
        start_line = line.split(":")
        if start_line[0] == "Coverage":
            update_test_coverage_in_readme()
            break
        if line == "# Project Title\n":
            add_test_coverage_to_readme()
            break
    file_input.close()

add_or_update()
```
</details>

## Task 1c: Rather than update code coverge use a shields.io 
Look at the following image: 
![coverage](https://img.shields.io/badge/coverage-34%25-888800)

The image uses the hyperlink https://img.shields.io/badge/coverage-34%25-888800.
It is possible to create personalised shields by sending informtion to the correct endpoint.

After /badge/ it takes in 3 arguments, separated by a hyphen. 
The first argument is the first text message.
The second argument is the second text message.
The third argument is the color of the badge.

Instead of updating the coverage as a piece of text, use a shield to represent the percentage

## Task 1d: Update the colour to match the percentage
The third colour argument should match that of the percentage. If the Test percentage is 0% it should be red (ff0000), and when it is 100% it should be green (00ff00). Create a sliding scale of colour from 0% to 100%

## Task 2: Add this to jenkins
Create a new jenkins freestyle project. And scroll down to source code management. Enter in the ssh connection such that jenkins can download using ssh keys. 
![scm](https://i.imgur.com/DD6lOP9.png)

**ERROR?** 
You might get errors if you have not created the ssh keys within the jenkins user. To change users to jenkins run:
```bash
sudo su - jenkins
```

THEN run the steps on how to create an ssh key and then put the key on github.

Next:

* Add the running of this python automation method

* You will need to push it back up to github

![build](https://i.imgur.com/kmOTU61.png)

Don't forget to poll the scm every minute.

Finally:
* On your local machine add the @Ignore annotations to test methods
* add them and commit then push to your hosted repository
* If everything has worked you should now have jenkins automatically applying changes to your github page.
