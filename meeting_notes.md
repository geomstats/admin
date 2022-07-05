Meeting Notes for Geomstats Monthly Meetings
============================================

Geomstats meetings take place the first Tuesday of each month, at 8.30 am PST.

Meeting: 2022/07/05
-------------------
Present: Luis, Anna, Xavier, Alice, Nicolas, Nina.

- Nina updates the group about the progress on the information geometry module (with fisher-rao metrics): collaborators Antoine (multivariate normal distributions), Jules Deschamps (lead) and Saiteja (dual connection structure) are working on this.
- Alice and Nina mention that, in the last week of July, Alice and Jules might be going to Maryland summer school to give tutorials on information geometry and geomstats.
- The group discusses the elastic shape module and the possible extensions to manifold-valued (not only R^d-valued) curves, e.g. curves defined on the space of SPD matrices in collaboration with Sylvain. The group discusses extensions of the SRV transform to this setting.
- Nina updates the group on the visualization module, from the design document started by Elodie, the PR submitted by Luis and the new additions by Jules.
- Anna gives updates on graph spaces and next iterations on the implementation of the AAC algorithm, versions of mean, PCA and regression.
- Anna mentions that the development of Wald spaces is postponed to september.
- Anna describes Aymeric's potential contribution to Geomstats' hypothesis testing suite. He might port permutation tests on metric spaces to Geomstats' (Riemannian) manifolds.
- Anna describes relevant research from the International symposium on nonparametric statistics (ISNPS), for example Frechet regression model on metric spaces that could be of interest to Geomstats.
- Xavier and Nicolas will go to Gaml (Geometric approaches in machine learning) meeting where Nicolas will give a tutorial session on Geomstats. https://gaml.mathi.uni-heidelberg.de/ 
- Luis updates the team on backends: robust way of testing backends: e.g. disambiguating matmul in vectorization scenarios, by introducing new matvec, or hiding einsum in the backends while solving a potential performance issue on einsum in numpy. Discussion about the meaning of "vectorization" depending on the community using it: in linear algebra versus in computer science. The goal is to hide the "vectorization" (computer science meaning) into the backends, as vectorizing computations is one of the main obstacles for new contributors.
- Luis discusses the dtype (flaot32, float64) in the backends. There is a global dtype (for np at least) and binary operators that take two dtypes can usually output the correct dtype. More tests are needed but the PR should be submitted soon.
- Luis discusses the contributions of Yann about introducing manifolds on the complex field. The group discusses manifold on other fields and agrees to keep them separated from the manifolds defined on the real field for now.
- The group discusses the importance of improving the tests for the learning algorithms, a portion of the library that has not been addressed by Saiteja's refactoring.
- Luis discusses the PR on Frechet mean. A question on whether we need "point_type" is raised: this could be replaced by computing the len() of the shape attribute.
- Luis proposed to put datasets on figshare (following scikit-learn), which generates a DOI for the dataset. We should move all data there (at least the biggest ones). We should not split train/test in the load functions.
- Alice updates the group about hiring a research engineer in Paris.


Meeting: 2022/06/07
-------------------
Present: Xavier, Luis, Anna, Alice, Nina.

- The group sets up agenda items.
- Alice presents the motivation and design behind refactoring solvers.
- The group talks about pros and cons of the design of the solvers.
- Nina describes Open Collective, NumFOCUS and CZI submitted applications.
- Xavier discusses the interactions of these structures for Geomstats which is a cross-continents organizations.
- Nina describes updates on governance and roadmap documents, the group adds items to the roadmap.
- Luis presents on-going work on backends, numerical precision (setting a global parameter across backends) and tests.
- Anna presents the strategy for the next months on the development of the stratified spaces.
- Luis and Nina presents updates on progress made at Google Season of Docs.

Meeting: 2022/05/03
-------------------

Present: Alice, Sophia, Anna, Nicolas, Nina.

- Everybody agrees to apply to NumFOCUS.
- Nicolas and Nina talk about refactoring the FiberBundle class, see [Issue 1480](https://github.com/geomstats/geomstats/issues/1480).
- Sophia presents future design of geometry module.
- Anna and Sophia discuss literature reviews in machine learning linked to Geomstats.
- Nina updates the group about Geomstats mailing lists: 
 - geomstats@googlegroups.com sends emails to community members interested in next Geomstats events such as hackathons.
 - team@geomstats.org sends emails to the core developers.
 - Alice, Anna, Nina and Saiteja agree to have team@geomstats.org forward messages to their email addresses.

Meeting: 2022/04/07
-------------------

In person, at the hackathon in Saint Raphael, France.
Present: Xavier, Nina, Alice, Anna, Nicolas.

- Xavier proposes to look for ideas to create Geomstats' foundation.
- Nina mentions NumFOCUS.
- Everybody discusses the organization of the next hackathon, in October in Paris, and collaborators to invites.
- Alice presents the scope of the semester "Geometry & Statistics" at the IHP, Paris in October.
