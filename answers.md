# Part 1 — Concepts & Theory
---
## Q1. Terminal basics

**1. What is a terminal? How is it different from clicking through folders?**    
A terminal is a text-based interface used to control and navigate a computer by typing text commands. Unlike GUI where you interact visually using a mouse, in CLI you have to type in commands for each task 

**2. Write the terminal command to:**

**(a) list all files including hidden ones**

ls -a (works in git bash)
but if its powershell, ls -a wont work. 
then use get-childitem -force

**(b) create a folder called project**    
mkdir project

**(c) move into that folder**    
cd project

**3. What does the cd .. command do? What about cd ~?**     
cd .. moves you one level back in the folder and cd~ brings you to home directory

## Q2. Version control concepts

**4. Explain what version control is in your own words. Give one real-world analogy (not Google Docs — pick your own).**     
Version control is basically about tracking andd logging the changes made to files over time. So when needed, previous version can be restored.        
Example: While editing a photo in Inshot, one can always go back to the previous filter or cropping or any change one has made. so Inshot tracks the changes in edit and helps in reverting them back

**5. What is the difference between Git and GitHub? Can you use Git without GitHub?**   
Git is a tool that runs on your local device and helps you track changes and maintain version history on your device. Github is a cloud platform on which people collaborate and store their repos.  
And yes, Git can be used without GitHub if you dont need to collaborate or share your code with anyone.

**6. Name three features GitHub provides that Git alone does not.**   
1. provides a GUI for reviewing merge requests, code reviews etc    
2. helps in collaboration over projects by multiple people    
3. deploying the code made easier

**7. What is a repository? What is the difference between a local repository and a remote repository?**     
A repository is a folder containing your project files along with the full history of changes tracked by Git. local repository resides on your physical computer whereas a remote repository is hosted on a remote server or cloud service like GitHub

## Q3. Git internals     

**8. Explain what HEAD means in Git. What happens to HEAD when you switch branches?**       
HEAD is a pointer that indicates your current active branch and working commit in git. When you switch branches, HEAD moves to that branch and point to the latest commit of that target branch.

**9. What is a .gitignore file? Write a .gitignore that ignores: all called node_modules/ , and any file ending in .env .**     
A .gitignore file specifies intentionally untracked files and folders that Git should ignore and never commit to history.     
Example:  .env , node_modules

# Part 2 — Core Commands Lab
---

## Exercise 2.2 — The staging area   

**6 . Explain: Why did we commit A+B separately from C? When would you want to split commits this way?**     
We committed A+B separately from C to keep our commit history logical and organized. Each commit should ideally represent a single logical unit of work or feature.   
This splitting of commits shouuld be done when:
- **Logical grouping:** Different features or fixes belong to different tasks, so keeping them in separate commits makes code reviews easier.
- **Selective rollbacks:** If feature C got a bug later on, you can revert or undo only feature C's commit without affecting features A and B.
- **Clearer history:** It makes the git history cleaner, making it much easier for teammates to understand exactly what each change introduced.  


## Exercise 2.3 — Push & pull    

**6. What would happen if you made a local commit first WITHOUT pulling? What command helps you check?**       
If you make a local commit before pulling the new commit from GitHub, your local repository and remote repository will diverge (they will both have new commits that the other doesn't have) When you try to push your local commit to GitHub later, Git will reject the push and require you to pull first, which may result in a merge conflict that you'll need to resolve.    
To check for remote changes before pulling or pushing, you can run git fetch followed by git status which shows how many commits your local branch is behind or ahead of the remote branch.

# Part 3 — Branching & Merging Lab 
---

## Exercise 3.1 — Branch basics 

**9.  Did this merge create a merge commit or was it a fast-forward? How can you tell?**       
This merge created a merge commit (not a fast-forward) coz:
1. In the terminal graph, two separate branch lines coming together into a single node with a message like *"Merge pull request..."*.
2. On GitHub, it explicitly shows a merge commit combining the history of feature/profile-page into main rather than just moving the pointer forward.

## Exercise 3.2 — Deliberate conflict 

**15 .Why did this conflict happen? (Reference the rule: same file, same line)**     
This conflict happened because both the main branch and the feature/update-motto branch edited the exact same line (line 2) of the exact same file team.txt. git cant automatically determine which motto version is correct so it pauses the merge and asks the user to resolve the conflict

**Would there have been a conflict if the two branches edited different lines of team.txt ?**     
No, there would not have been a conflict. changes will be on different lines so wont overlap and wont create a conflict.

**Would there have been a conflict if one branch edited team.txt annd the other edited about.md ?**      
No, there would not have been a conflict. coz these 2 are completely separate files and the changes wont overlap

# Part 4 — GitHub Workflow Challenge

## Exercise 4.1 — Feature branch workflow 

**13.  What is the purpose of a pull request? Why not just merge directly?**     
A Pull Request is created so the lead or senior developer can review and test the code before it enters the main branch. Merging directly bypasses code reviews. This increases the risk of pushing broken code or bugs straight into the project

**Name two things a reviewer should check in a pull request.**    
Conflicts(if there arises a conflict, reviewer needs to identify which code to keep and what to remove) 
Code Correctness & Logic (no bugs and should meet the requirement of what was intended)


**What is the difference between a merge commit and a fast-forward merge?**        
Merge Commit: Created when branches have diverged, creating a brand-new "commit" node that explicitly ties both historical branches together.         
Fast-Forward Merge: Occurs when the target branch has no new commits since branching, simply moving the pointer forward to the latest commit without creating an extra merge node.

# Part 5 — Merge Conflict Scenarios

## Scenario A 

No, there will not be a conflict. coz Ahmed and Sara edited completely different lines of the file (line 10 vs. line 1) even if the file was same (index.html). this allows Git to automatically merge both without nay conflict

## Scenario B

No, there will not be a conflict. coz Ali and Fatima are working on entirely different files. Git can easily merge them coz there's no overlapping at all

## Scenario C

Yes, there will be a conflict. coz both developers changed the exact same line (line 5) of the same file (config.py) to conflicting values. now Git has to pause and ask for conflict resolution.

```
<<<<<<HEAD (current change)    
DEBUG = True      
=======        
DEBUG = False            
>>>>>> (incoming change) 
```     

## Scenario D

1. Open index.html in VS Code and locate the conflict markers     
2. Edit the file to keep the desired code changes so that all conflict marker lines are deleted entirely     
3. Run git add index.html to stage the fix           
4. Run git commit -m 'resolve merge conflict in index.html'            
5. Now merge it and it wont be conflicted            

## Scenario E

Yes, this is an excellent practice coz it ensures you always start your work with the most up-to-date version of the codebase shared by your team.   
If you skip it, you will be working on outdated code or might do the work which your teammates have already done. later, it will result in merge conflicts as well