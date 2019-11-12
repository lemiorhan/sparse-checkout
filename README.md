# EASY SPARSE CHECKOUT FUNCTIONALTY 

If you are working with mono repos and if you are not at the beginning of your product, you may need to deal with huge number of files in your IDE. Opening that number of files might cause high memory usage, long indexing time and a lot of wait times while working at your IDE.

[Sparse checkout](https://git-scm.com/docs/git-read-tree) simply help you work on the folders you really need to work on and dismiss all the others. It simply makes huge mono repos managable by your IDE.

## Installation

1. Clone the project to a folder you desire. 
2. Open `sparse-checkout` script and add the folders you need to keep to `FOLDERS_TO_KEEP` variable.
3. Be sure that you have execution rights of the script. If you cannot execute it, assign permission by `chmod sparse-checkout 750` command.

## CLI Options

After succesfull installation, you can display the options as follows.

### Applying

```
sparse-checkout apply FOLDER_NAME_1 FOLDER_NAME_2 FOLDER_NAME_3 [FOLDER_NAME_N]
```
You can add any number of folder names you need to keep. Please note that the list of folder names assigned to `FOLDERS_TO_KEEP` variable are the ones which will be kept by default. By giving a folder name as the parameter, you will be able to keep dynamically, as you wish.

Please also note that the files in the root folder are kept too. When you apply sparse checkout, you won't lose unkept folders. They are still kept by git inside .git folder as compressed objects. You only don't see them at working copy as source code. `git status` does not mention any deletion, don't worry. You will still have access to all commits. You can still use all git commands safely.

### Resetting
```
sparse-checkout reset
```
Simply resets sparse checkout and makes all folders accessible again.

## How It Works

Sparse checkout requires a few steps to be applied.
```
git config core.sparsecheckout true
echo "/*" >.git/info/sparse-checkout
echo "!FOLDER_TO_REMOVE" >>.git/info/sparse-checkout
git read-tree -mu HEAD
```
Folders to keep and remove should be put into `.git/info/sparse-checkout` file. Therefore we add `/*` as the first line to keep files in the root folder and add each folder to be removed one by one by putting exclamation mark to the begining of each line. `git read-tree` makes the magic.

The script resets sparse-checkout before applies it. Normally when you call the script to keep FOLDER_A when sparse checkout has already been applied before for FOLDER_B, the script fails. Resetting before applying makes it safer.
