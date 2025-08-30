# The Entity-Relationship Model—Toward a Unified View of Data

## PETER PIN-SHAN CHEN - Massachusetts Institute of Technology

A data model, called the entity-relationship model, is proposed. This model incorporates some of the important semantic information about the real world. A special diagrammatic technique is introduced as a tool for database design. An example of a database design and description using the model and the diagrammatic technique is given. Some implications for data integrity, information retrieval, and data manipulation are discussed.

**Key Words and Phrases:** database design, logical view of data, semantics of data, data models, entity-relationship model, relational model, Data Base Task Group, network model, entity set model, data definition and manipulation, data integrity and consistency

**CR Categories:** 3.50, 3.70, 4.33, 4.34

---

# 1. INTRODUCTION

The logical view of data has been an important issue in recent years. Three major data models have been proposed: the network model [2, 3, 7], the relational model [8], and the entity set model [25]. These models have their own strengths and weaknesses. The network model provides a more natural view of data by separating entities and relationships (to a certain extent), but its capability to achieve data independence has been challenged [8]. The relational model is based on relational theory and can achieve a high degree of data independence, but it may lose some important semantic information about the real world [12, 15, 23]. The entity set model, which is based on set theory, also achieves a high degree of data independence, but its viewing of values such as “3” or “red” may not be natural to some people [25].

This paper presents the entity-relationship model, which has most of the advantages of the above three models. The entity-relationship model adopts the more natural view that the real world consists of entities and relationships. It

---

Copyright © 1976, Association for Computing Machinery, Inc. General permission to republish, but not for profit, all or part of this material is granted provided that ACM's copyright notice is given and that reference is made to the publication, to its date of issue, and to the fact that reprinting privileges were granted by permission of the Association for Computing Machinery. A version of this paper was presented at the International Conference on Very Large Data Bases, Framingham, Mass., Sept. 22-24, 1975.

Author's address: Center for Information System Research, Alfred P. Sloan School of Management, Massachusetts Institute of Technology, Cambridge, MA 02139.

ACM Transactions on Database Systems, Vol. 1, No. 1, March 1976, Pages 9-36.

---

incorporates some of the important semantic information about the real world (other work in database semantics can be found in [1, 12, 15, 21, 23, and 29]). The model can achieve a high degree of data independence and is based on set theory and relation theory.

The entity-relationship model can be used as a basis for a unified view of data. Most work in the past has emphasized the difference between the network model and the relational model [22]. Recently, several attempts have been made to reduce the differences of the three data models [4, 19, 26, 30, 31]. This paper uses the entity-relationship model as a framework from which the three existing data models may be derived. The reader may view the entity-relationship model as a generalization or extension of existing models.

This paper is organized into three parts (Sections 2-4). Section 2 introduces the entity-relationship model using a framework of multilevel views of data. Section 3 describes the semantic information in the model and its implications for data description and data manipulation. A special diagrammatic technique, the entity-relationship diagram, is introduced as a tool for database design. Section 4 analyzes the network model, the relational model, and the entity set model, and describes how they may be derived from the entity-relationship model.

---

2. THE ENTITY-RELATIONSHIP MODEL

2.1 Multilevel Views of Data

In the study of a data model, we should identify the levels of logical views of data with which the model is concerned. Extending the framework developed in [18, 25], we can identify four levels of views of data (Figure 1):1

(1) Information concerning entities and relationships which exist in our minds.2

(2) Information structure—organization of i3nformation in which entities and relationships are represented by data.4

(3) Access-path-independent data structure—the data structures which are not involved with search schemes, indexing schemes, etc.5

(4) Access-path-dependent data structu6re.

In the following sections, we shall develop the entity-relationship model step by step for the first two levels. As we shall see later in the paper, the network model, as currently implemented, is mainly concerned with level 4; the relational model is mainly concerned with levels 3 and 2; the entity set model is mainly concerned with levels 1 and 2.

2.2 Information Concerning Entities and Relationships (Level 1)

At this level we consider entities and relationships. An entity is a “thing” which can be distinctly identified. A specific person, company, or event is an example of an entity. A *relationship* is an association among entities. For instance, “father-son” is a relationship between two “person” entities.¹

---

¹It is possible that some people may view something (e.g., marriage) as an entity while other people may view it as a relationship. We think that this is a decision which has to be made by the enterprise administrator [27]. He should define what are entit7ies and what are relationships so that the distinction is suitable for his environment.

ACM Transactions on Database Systems, Vol. 1, No. 1, March 1976.

---



---

Fig. 1. Analysis of data models using multiple levels of logical views

The database of an enterprise contains relevant information concerning entities and relationships in which the enterprise is interested. A complete description of an entity or relationship may not be recorded in the database of an enterprise. It is impossible (and, perhaps, unnecessary) to record every potentially available piece of information about entities and relationships. From now on, we shall consider only the entities and relationships (and the information concerning them) which are to enter into the design of a database.

2.2.1 Entity and Entity Set. Let *E* denote an entity which exists in our minds. Entities are classified into different **entity sets** such as **EMPLOYEE**, **PROJECT**, and **DEPARTMENT**. There is a predicate associated with each entity set to test whether an entity belongs to it. For example, if we know an entity is in the entity set **EMPLOYEE**, then we know that it has the properties common to the other entities in the entity set **EMPLOYEE**. Among these properties is th1e aforementioned test predicate. Let *E*ᵢ denote entity sets. Note that entity sets may not be mutually disjoint. For example, an entity which belongs to the entity set **MALE-PERSON** also belongs to the entity set **PERSON**. In this case, **MALE-PERSON** is a subset of **PERSON**.2

2.2.2 Relationship, Role, and Relationship Set. Consider associations among entities. A relationship se3t, *R*ᵢ, is a mathematical relation [5] among *n* entities,

---

ACM Transactions on Database Systems, Vol. 1, No. 1, March 1976.
