Meeting Notes for Geomstats Monthly Meetings
============================================

Geomstats meetings take place the first Tuesday of each month, at 8.30 am PST.


Meeting: 2023/05/02
-------------------
Present: Luis, Xavier, Souhail, Alice, Nina.
- Souhail updates the group on refactoring discrete curves, so that it inherits from PullbackDiffeoMetric.
- Nina updates the group on the discrete surfaces PR.
- FiberBundle updates.
- Solvers updates.


Meeting: 2023/04/18
-------------------
Present: Anna, Tra My, Luis, Souhail, Nina, Alice, Yann.
- Anna & Luis have started working on graph spaces again, will update next time.
- Souhail updates the group on the dynamic programming algorithm on discrete curves, its complexity and precision properties. PR coming soon.
- Tra My updates the group on the solvers, esp. Log solver, with tests. PR this week!
- Luis discusses the "Refactor metric to receive space" PR. Everything is working, no new tests have been skipped. Missing feedback on fiber bundle. 

Option 1 (Alice): One fiber bundle class:
- with equip_with_action giving default_action
-> defines what is the fiber, i.e. vertical space
- default with ._metric that is the total metric on the total space, can change with equip_with_metric.
-> defines what is the horizontal space that depends on ._metric
- and ._quotient_metric that is one corresponding to the quotient metric for that action.
- and ._quotient (base) that is a horizontal section of the bundle, for the ._metric

Option 1bis: One fiber bundle class and a quotient space class:
class FiberBundle:
- equip_with_action gives ._action , default action that can be changed
-> defines what is the fiber, i.e. vertical space
- equip_with_metric gives ._metric , default metric that can be changed
-> defines what is the horizontal space that depends on ._metric
- Need to enforce that action and metric are compatible.
Remark: does not know anything about its base

class QuotientSpace: (the former .base)
- has a .fiberbundle input with compatible action and metric
- has a ._metric that is the corresponding quotient metric

Remark: base space = space of all the fibers

Note we have two main scenarios with FiberBundles:
- Scenario 1: we have base explicit + total: example = full rank correlation matrices
define same fiber bundle but also another space for the base space.
- Scenario 2: when base is not known: example = kendall shape space
Option 1 is nice for that scenario.

Current: 
- PreShape becomes: PreShapeSpace (total, nothing bundle, receives quotient metric) PreShapeBundle (total space inherits fiber bundle)
Quick Correction:
- PreShape becomes: PreShapeSpace (total, nothing bundle) PreShapeBundle (total space inherits fiber bundle, receives quotient metric)
An alternative: 
- PreShapeSpace = PreShapeSpaceBundle is the space before quotienting, should be the total space with total metric, add the bundle structure there.
- ShapeSpace = SpaceSpaceQuotient is the space after quotienting, should have the quotient metric.

Option 2: one fiber bundle per action: RotationBundle

- Do we want to organize a challenge for TAG-ML at ICML Hawaii? Or any event associated with it? No.
- New infrastructure? pyproject.toml is the new standard for packaging python packages, setup.py is now deprecated 
- Need a plan to organize releases, let's release after the big PR.
- Luis updates the group on the tests, some fixes (esp. vectorization) will come after the big PR.

Meeting: 2023/03/14
-------------------
Present: Alice, Souhail, Anna, Luis, Yann, Nina, Xavier.

- We have two free registrations for our GSI session: Luis and Nicolas will sync with Frederic and Xavier to organize.
- Nina updates the group on the homework submitted by her students on visualizing manifolds.
- Souhail & Alice update the group regarding the discrete curves refactor using the pullback diffeo metric: `SRVMetric`, `ElasticMetric`.
- Anna updates the group regarding graph spaces, BHV space and Wald space. Xavier & Anna are writing a software report with the current examples.
- Luis updates the group on recent progress refactoring the metric: metric takes the metric as input.
- Luis updates the group on the work with Tra My on the solvers: she is vectorizing the log solver; Yann has an alternative implementation in pytorch.

Meeting: 2023/02/07
-------------------
Present: Xavier, Anna, Tra My, John, Yann, Luis, Nina.

- Tests: test on geometry + vectorization tests are rewritten for manifolds (not for metrics): we cover 50%. Luis will push a PR with only fixes that emerged from the tests. Next step: do it for the metrics. Difficulty: structure for connection / metrics are not good, pb with extrinsic/intrinsic coordinates: the idea of equiping so that metric receives space will help and we need to move fast on this. Note: Morten implements pga for gsi -> shows how important it is to pass the space to the metric.
- Geomstats tool for tests: experiment by Luis, which automatizes the writing of tests. It allows to get all methods of a space and see which tests are missing, by leveraging namings of methods. It is a standalone tool for now.
- Conversion method are currently within the manifold -> move them out, which simplifies the tests. Proposal: an object that transforms coordinates: discussion on whether it should be: a standalone SphereTransform object, or go in the interface? Problem: how do we know what is the input to the conversion method? Changing from one chart to another is different from changing from a coord system to another? Problem: explosion in the number of manifolds: manifolds x charts. 
- Immersed set implementation (Morten), requires immersion, then everything else comes from automatic diff. The group discusses whether OpenSet should inherit from ImmersedSet? We might not get anything from it except for code complexity. Note that, for LevelSet, when we go to higher codimensions, code might not work as well (when the level in LevelSet is not a value, but is of higher dimensions: add it in docs).
- Numerics in solvers: Tra My & Luis are working on this, difficulty: needs changes in connection; blocked on manifold.equip and tests PRs. Will need to discuss integrators in geomstats, because we are underperforming against scipy which uses adaptive steps. (Ours are automatically differentiable -> important for learning). 
- Fix: Some methods are not differentiable; e.g. broadcast arrays. -> need to find solutions to have learning algorithms work.
- Information geometry: Tra My implemented distributions that are 1D: Geometric, Poisson, Binomial (closed forms solutions) and Exponential + modified Beta, Gamma because point_to_pdf was not working well + vectorized the FisherRaoMetric. NaN values in exp of FisherRao in pytorch, log, geodesics in autograd fail --> these errors could go away with adaptive optimization.
- Future events? 1) Sprint to merge PRs, 2) Amsterdam SIAM conference: Luis presents Geomstats with Yann T., 3) GSI session: 30th august, Sept 1st?


Meeting: 2022/12/06
-------------------
Present: Tra My, Alice, Luis, Nina.

- The group discusses the successful NumFOCUS application (Affiliated Project status).
- The group discusses end of google season of code. While it may be hard to enter the project, this internship created great interactions with python core community. We need to upgrade to Python 3.10 or 3.11
- The group discusses the last hackathon, which involved great new contributors. 
- The group discusses survey on how participants liked the hackathons, which can be sent at the end of the next one.
- The group discusses removing TF (Johan offered to do it). For example, TF's reshape does a copy, and not a view, which slows down the library. We need to verify whether they have a view that we can wrap as reshape. We decided to keep TF but be nice with contributors when it fails.
- The group talks about using Numba to speedup numpy, specifically for integrators.
- Nina updates the group about the neurips workshop: geometry in neural representations.
- New members have or will join the team: a new intern in Paris/UCSB on the shape module, who starts in Feb and a new engineer in Parisalso on the shape module.
- The group discusses possible next hackathons (the one at the SIAM CSE conference may not happen): a smaller hackathon with new intern in Feb, another one in Paris with Yann end of June, depending on funding possibly another one in the US later next year.
- Luis presents the strategy for the new test structures: use pytest more (not hacking pytest' parameterizer, marks)
- The group agrees about the need to discuss complex manifolds.


Meeting: 2022/11/08
-------------------
Present: Anna, Nina.
- Anna and Nina discuss the integration of the stratified spaces within the manifold structure.

Meeting: 2022/10/11
-------------------
This meeting was replaced by conversations during the hackathon at the IHP. [The summary of the conversations can be found here](https://github.com/geomstats/geomstats/pull/1716).

Meeting: 2022/09/06
-------------------
Present: Luis, Anna, Xavier, Nina, Alice.
- Nina updates the group on the CZI grant application.
- The group discusses the logistics of the next hackathon in October in Paris. Alice mentions that we have a room at the IHP, for 30 people, whole week.
- Alice and Nina update the group about the information geometry module, which is almost completed but misses multivariate non-centered normals.
- Anna updates the group about the stratified space module.
- Luis updates the group about progress made on flag manifolds with Tom, which makes the connexion with pymanopt. 
- The bridge with pymanopt does not work anymore, because the pymanopt's API changed. We might need to implement our own optimization methods.
- Luis updates the group about new statistical tests, and estimators, that are being implemented.
- Geomstats team mailing list does not work.
- Luis explains the PR on the global dtype, that will solve the tolerance issues in Geomstats. It could be complex to understand. It will need to be made consistent with complex numbers.
- Luis updates the group the work on the new solvers, with Alice.
- Luis tells the group that we should use the latest versions of tensorflow and pytorch.

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
