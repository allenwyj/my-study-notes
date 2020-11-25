# save() and persist() - parent/child

- When using fine-grained cascade types..
    - cascade={CascadeType.DETACH,CascadeType.MERGE, CascadeType.PERSIST,CascadeType.REFRESH}
- Then you can use another method to persist everything in a single statement.
    - You can use: **session.persist(tempCourse);**
- When using **persist(..)**, you do not have to save child items individually. just save parent item and everything else is saved.

    ```java
    Student tempStudent1=new Student("Ramesh","Chikoti","chikoti@gmail.com");
    Student tempStudent2=new Student("Gautam","Korikar","Korikar@gmail.com");   
    Student tempStudent3=new Student("Eshwara","Bolem","Bolem@gmail.com"); 
    tempCourse.addStudent(tempStudent1);    
    tempCourse.addStudent(tempStudent2);   
    tempCourse.addStudent(tempStudent3);  
    System.out.println("Saving students!");  
    //session.save(tempStudent1);      
    //session.save(tempStudent2);     
    //session.save(tempStudent3);  
    session.persist(tempCourse);
    ```

- 
- you can use `session.save(tempCourse)` when you use CascadeType.ALL.
- But, when using fine-grained cascade types like below, `session.save(tempCourse)` won't work. Instead you can use `session.persist(tempCourse)`
    - cascade={CascadeType.DETACH,CascadeType.MERGE, CascadeType.PERSIST,CascadeType.REFRESH}
- Both save and persist trigger an entity state transition from the transient state to managed. However, `persist` is supported by JPA, while `save` is only supported by Hibernate. Both have different implementations.
    - `persist` doesn't guarantee that the identifier value will be assigned to the persistent instance immediately, **the assignment might happen at flush time**. Which means, it will not execute an INSERT statement if it is called outside of transaction boundaries. This is useful in long-running conversations with an extended Session/persistence context.
    - `save` does not guarantee the same, **it returns an identifier, and if an INSERT has to be executed to get the identifier (e.g. "identity" generator, not "sequence"), this INSERT happens immediately,** no matter if you are inside or outside of a transaction. This is not good in a long-running conversation with an extended Session/persistence context.
- These differences makes that we need to use `session.persist(..)` when we use fine-grained cascade types.
- Persist() is preferred over save().

[Hibernate's persist(), save(), merge() and update() - Which one should you use?](https://thorben-janssen.com/persist-save-merge-saveorupdate-whats-difference-one-use/)