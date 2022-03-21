---
description: LeetCode588
---

# Design In-Memory File System

{% embed url="https://leetcode.com/problems/design-in-memory-file-system" %}



Design a data structure that simulates an in-memory file system.

Implement the FileSystem class:

* `FileSystem()` Initializes the object of the system.
*   `List<String> ls(String path)`

    * If `path` is a file path, returns a list that only contains this file's name.
    * If `path` is a directory path, returns the list of file and directory names **in this directory**.

    The answer should in **lexicographic order**.
* `void mkdir(String path)` Makes a new directory according to the given `path`. The given directory path does not exist. If the middle directories in the path do not exist, you should create them as well.
* `void addContentToFile(String filePath, String content)`
  * If `filePath` does not exist, creates that file containing given `content`.
  * If `filePath` already exists, appends the given `content` to original content.
* `String readContentFromFile(String filePath)` Returns the content in the file at `filePath`.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/28/filesystem.png)

```
Input
["FileSystem", "ls", "mkdir", "addContentToFile", "ls", "readContentFromFile"]
[[], ["/"], ["/a/b/c"], ["/a/b/c/d", "hello"], ["/"], ["/a/b/c/d"]]
Output
[null, [], null, null, ["a"], "hello"]

Explanation
FileSystem fileSystem = new FileSystem();
fileSystem.ls("/");                         // return []
fileSystem.mkdir("/a/b/c");
fileSystem.addContentToFile("/a/b/c/d", "hello");
fileSystem.ls("/");                         // return ["a"]
fileSystem.readContentFromFile("/a/b/c/d"); // return "hello"
```

&#x20;

**Constraints:**

* `1 <= path.length, filePath.length <= 100`
* `path` and `filePath` are absolute paths which begin with `'/'` and do not end with `'/'` except that the path is just `"/"`.
* You can assume that all directory names and file names only contain lowercase letters, and the same names will not exist in the same directory.
* You can assume that all operations will be passed valid parameters, and users will not attempt to retrieve file content or list a directory or file that does not exist.
* `1 <= content.length <= 50`
* At most `300` calls will be made to `ls`, `mkdir`, `addContentToFile`, and `readContentFromFile`.

```java
class FileSystem {
    
    private class FileNode {
        private TreeMap<String, FileNode> map;
        private StringBuilder file;
        private String name;
        
        public FileNode(String name) {
            this.name = name;
            map = new TreeMap<>();
            file = new StringBuilder();
        }
        
        public String getName() {
            return name;
        }
        
        public boolean isFile(){
            return file.length() > 0;
        }
        
        public List<String> getList(){
            List<String> list = new ArrayList<>();
            if (isFile()) {
                list.add(getName());
            } else {
                list.addAll(map.keySet());
            }
            
            return list;
        }
        
        public void addContent(String content) {
            file.append(content);
            return;
        }
        
        public String readContent() {
            return file.toString();
        }
    }
    
    private FileNode root;
    public FileSystem() {
        root = new FileNode("");
    }
    
    public List<String> ls(String path) {
        return findNode(path).getList();
    }
    
    public void mkdir(String path) {
        findNode(path);
        return;
    }
    
    public void addContentToFile(String filePath, String content) {
        findNode(filePath).addContent(content);
        return;
    }
    
    public String readContentFromFile(String filePath) {
        return findNode(filePath).readContent();
    }
    
    private FileNode findNode(String path) {
        String[] seq = path.split("/");
        FileNode cur = root;
        for (String file : seq) {
            if (file.length() == 0) {
                continue;
            }
            cur.map.putIfAbsent(file, new FileNode(file));
            cur = cur.map.get(file);
            if (cur.isFile()) break;
        }
        
        return cur;
    }
}

/**
 * Your FileSystem object will be instantiated and called as such:
 * FileSystem obj = new FileSystem();
 * List<String> param_1 = obj.ls(path);
 * obj.mkdir(path);
 * obj.addContentToFile(filePath,content);
 * String param_4 = obj.readContentFromFile(filePath);
 */
```
