---
title: Enumerating @NamedQuery within @NamedQueries
date: 2021-06-27
tags: [annotations, java]
---

![](https://res.cloudinary.com/davidsalter/image/upload/v1628716786/0_z_-_ubcmlLDmWWwe_adrqiz.jpg)

## Introduction

If you're a Java developer using JPA, chances are that you've declared one or more `@NamedQuery` objects on your entities.

To declare a `@NamedQuery` on a class, the class must simply be annotated with the name of the query and its JPQL, such as:

```java
@Entity
@NamedQuery(name = "findAllProjects",
			query = "select p from Project p order by p.id")
public class Project
```

If however, we wish to declare multiple `@NamedQuery` annotations, we annotate the class with a `@NamedQueries` annotation which then contains a collection of `@NamedQuery` annotations as follows:

```java
@Entity
@NamedQueries({
	@NamedQuery(name = "findAllProjects",
				query = "select p from Project p order by p.id"),
	@NamedQuery(name = "findById",
				query = "select p from Project p where p.id=:id")
})
public class Project
```

## Enumerating the `@NamedQuery` annotations

Once you've created an entity with multiple `@NamedQuery` annotations, how can you check what annotations are present on the class?

Fortunately using reflection, its a fairly simple matter to enumerate the annotations on the class and find the details about them as shown in the following code.

```java
NamedQueries annotation = Project.class.getAnnotation(
											NamedQueries.class
													 );
for (Annotation annot : annotation.value()) {
  System.out.println(annot.toString());
  for (Method method : annot.annotationType().getDeclaredMethods()) {
    if (method.getName().equalsIgnoreCase("name") ||
        method.getName().equalsIgnoreCase("query")) {
		try {
        String result = method.getName() +
	                    " : " +
                        method.invoke(annot,  null).toString();
        System.out.println(result);
      } catch (IllegalAccessException | IllegalArgumentException
			| InvocationTargetException e) {
        // Oops - something has gone wrong.
		break;
      }
    }
  }
}
```

Running the above code produces the following output:

```bash
@javax.persistence.NamedQuery(lockMode=NONE, hints=[], name=findAllProjects, query=select p from Project p order by p.id)
name : findAllProjects
query : select p from Project p order by p.id

@javax.persistence.NamedQuery(lockMode=NONE, hints=[], name=findById, query=select p from Project p where p.id=:id)
name : findById
query : select p from Project p where p.id=:id
```

## Credits

Photo by [Xavier von Erlach](https://unsplash.com/@xavier_von_erlach?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&
