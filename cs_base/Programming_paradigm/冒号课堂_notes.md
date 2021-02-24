- 通过给文件类各添加一个`accept`方法

<!--language: !csharp-->

    using System;
    using System.Collections.Generic;
    using System.Linq;

    abstract class File {
        public string Name {get; set;}
        abstract public void Accept(FileVistor visitor);
    }

    class PlainFile : File{
        override public void Accept(FileVistor visitor){visitor.Visit(this);}
    }

    class Directory : File{
        override public void Accept(FileVistor visitor){visitor.Visit(this);}
    }

    interface FileVistor{
        void Visit(PlainFile file);
        void Visit(Directory file);
        // void Visit(File file);
    }

    class FilePrinter : FileVistor{
        public void Visit(PlainFile file){
            Console.WriteLine("plain file: " + file.Name);
        }
        public void Visit(Directory file){
            Console.WriteLine("directory: " + file.Name);
        }
        public void Visit(File file){
            Console.WriteLine("abstract file: " + file.Name);
        }
    }

    class Program {
        static void Main(string[] args) {
            File dir = new Directory{Name = "directory_name"};
            File file = new PlainFile{Name = "file_name"};
            FileVistor visitor = new FilePrinter();

            dir.Accept(visitor); // equals: visitor.Visit((Directory)dir);
            file.Accept(visitor);// equals: visitor.Visit((PlainFile)file);
        }
    }

- `void Visit(File file);`也没必要存在了
- 在语句`file.accept(visitor)`中，通过本身的单分派找到实际的`accept`方法，实现了`file`的多态（类型多态）；接着在相应的`visitor.Visit(this)`中，再利用单分派找到实际的`Visit`方法，实现了`Visitor`的多态（参数多态）

![prog_paradigm_visitor](../../imgs/prog_paradigm_visitor.png)

- 缺陷是`Visitor`需要了解`File`的类型层级，可参考ACYCLIC VISITOR模式

### 迭代器模式
![prog_paradigm_iterator](../../imgs/prog_paradigm_iterator.png)

- 赋予了用户自定义循环的能力，实现了迭代抽象(iteration abstraction)，用户可以对聚合体或容器以某种次序遍历(traverse)容器中的元素，至于容器的内部结构、遍历的起止、推进等细节，均毫不知情，也不关心
- 迭代器作为容器与算法之间的中介，__促使算法摆脱了对数据结构的依赖__，从而更具普适性和重用性，一旦算法被抽象出来，__泛型范式__ 便可大展神威
- 泛型范式与迭代器相得益彰，充分体现在C++的STL、Java和C#的Collections中
