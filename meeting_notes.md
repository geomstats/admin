Meeting Notes for Geomstats Monthly Meetings
============================================

Geomstats meetings take place the first Tuesday of each month, at 8.30 am PST.

Meeting: 2024/07/02
-------------------
Present: Xavier, Luis, Nina
- Update on metrics on correlation matrices.
- Update on advanced information geometry: need to brainstorm on how the different structures interact, in the context of bregman divergences, dual connections, etc.
- Example of interacting structures:
  - In theorem: Statistical manifold needs metric tensor and 3rd-order tensor --> induces a dual connection structure wrt another, and wrt the metric
  - In definition: metric + compatible dual connection --> 3rd-order tensor
- Example of interacting structures: Riemannian metric:
  - if we have the inner_product (which comes with an implicit choice of basis for its input tangent vectors -- possibly basis of the embedding space), we should be able to compute the metric_matrix, if we know on a intrinsic basis of the manifold's tangent space. Maybe that's done for ImmersedManifolds only, since this is where we have these basis defined.
- Example of interacting structures: Subriemannian manifold:
  - Structure can be created from two inits.
- Improve the design of Lie groups : take a HOWTO approach. On Lie groups, the only cases where we have explicit geodesic equations are for bi-invariant metrics. Everything else needs to be done via integration (with ad*). Nicolas, on SE(3), took the classic bi-invariant metric on SO(3) and a non-isotropic metric on R3, and he might have been able to get an explicit equation.
- Need to go through notebooks taking a user approach, and add documentation.
- Need to go through remaining TODOs, eg, for the cone metric.


Meeting: 2024/06/04
-------------------
Present: Luis, Alice, Xavier, Pablo, Nina
- Update on stratified spaces module by Luis.
- Update on shape module:
   - 1) Nina: Add paragraph in each section explaining the limitations of current implementations, and what research needs to be done to further improve stability and speed.
   - 2) Nina: Add explanation in the docstrings of the quotient space structure that explains that quotient spaces are not manifold, but the principal stratum is a manifold.
   - 3) Nina: Add explanation in the docstrings of the GroupComposition Python class: be upfront about the fact that it's only an approximation when groups do not commute -- highlight as an important future research direction.
   - 4) Optional for the paper: Add unparameterized surfaces, but do not try to optimize decimation of meshes: instead, leave a note that the code is not stable nor fast -- highlight as an important future research direction.
   - 5) Nina: Mention the fact that FréchetMean in the quotient space is biased -- highlight as an important future research direction.
   - 6) Think about how to improve DiscreteCurves APIs: how do we instantiate DiscreteCurves with the L2 metric?
   - 7) Nina: Say we don't have the action of rotations on DiscreteSurfaces -- highlight that it's a complicated issue, eg, what should be the center of rotations?
   - 8) Luis: Use the same examples as Emmanuel's, but highlight i) improved readability, with consistency with the rest of the code + ii) performance speeds-ups: we are much faster doing the inner-product, which is the cornerstone of that part of the code.
   - 9) Alice: Highlight future research direction: discrete curves on Lie groups, thanks to geomstats' design.
   - 10) Optional for the paper: Luis/Alice: Implement the cone metric: right now, we only have it for restrictions on a, b parameters. Find the geodesic equation in closed form.
   - 11) Luis: Make 3 missing architecture diagrams.
   - 12) Leave varifolds out of this.
- Update on CIRM workshop.: Luis presented Geomstats.
- Update on advanced information geometry module with divergences and dual connections:
   - 1) Need to add tests,
   - 2) Need clean up the design,
   - 3) Need to take user perspective and write user's code before we design the core codebase.
- Update on visualization: add vector fields on the fields, thanks to on-going numerical regression work of Pablo with poisson-geometry module.
- Would be good to have a symbolic version of computations on manifolds. Different backend? Could we do it with sympy?

Meeting: 2024/04/02
-------------------
Present: Malik, Trey, Daniel, Luis, Pablo, Nina.
- Discuss the status of the shape module. We need 1) to implement SRV transform can be used for any a, b combination on curve shapes in any R^d, 2) fix the dynamic programming algorithm for reparameterization of curve, 3) implement varifold distances.

Meeting: 2024/03/05
-------------------
Present: Xavier, Luis, Pablo, Nina.
- change of coordinates: distinguish between diffeomorphism-case (intrinsic to intrinsic) and immersion-case (intrinsic to extrinsic)
- wald space is getting ready to merge! geometry-part only, more discussion needed on how to use learning algorithms there.
- discussion on the aligners on discrete curves: dynamic programming seems to loose the notion of shape, is this a problem in our code?
- speed: dynamic programming does not scale with the number of sampling points, our new symmetric aligner is independent of this number: much faster. Though: what's the shape we obtain?
- discretization from continuous curves to discrete: what information can we hope to model/recover with our discretization?
- updates on s2 with charts to bring in symplectic geometry
- updates on bregman divergence for the next iteration of the information geometry module.

Meeting: 2024/01/02
-------------------
Present: Xavier, Luis, Nina.
- Luis is starting in Italy! with SB in september, potentially an intern joining at that time
- Shape module project: discuss links with latest works from Emmanuel Hartman, Barbara Tumpach (curve reparameterization with higher sampling on high curvature regions), Jean Feydy (deformation of the space).
  - Todo: Generalize to curves & surfaces in higher-dimensions, or on higher-dimensional manifolds.
  - Todo: Add reparameterization of discrete surfaces with support for pykeops (optional requirement, not a backend), from Emmanuel Hartman's code.
  - We can leave out the diffeomorphisms implementation for now: make it a standalone, bigger project.
- Next step: add the implementation of the SVF, at least the exp and the log, in geomstats spirit, using Jean Feydy's pykeops and shape-ot.
- New poisson geometry module project: Pablo Suárez-Serrato
- New information geometry module (dual connection, bergman divergences using the code in java from Frank Nielsen) project: Pablo Suárez-Serrato
- Mathematical structures: Note that we can do space.dist(p1, p2) with current approach. we have name clashing for group.exp() (Cartan-Shouten connection) and metric.exp() for groups, we need a priority rule.
Divergence (=Dual Connections Flat? Note: only for convex spaces) --> Length Metric (with dist() method) --> Riemannian Metric (with dist() method)
Connection --> Riemanian Metric (=Levi-Civita Connection --> get the dual connections from an additional tensor)
Link with Hessian geometry, metric = hessian of a convex function
- We need to plan the next hackathon: March? in September in UCSB?


Meeting: 2023/11/07
-------------------
Present: Malik, Xavier, Luis, Nina.
- Malik updates the group on the discrete surfaces code, specifically the application of the log solver.
  - 1. Amil's issue is almost solved: frechet_mean.fit(one_discrete_surface) gives an error when you give an array of shape [n_vertices, 3], because it assumes that the axis 0 is the n_samples
  - --> should be of shape [n_samples, n_vertices, 3]
  - --> needs at least two surfaces, or an array [1, n_vertices, 3]
  - TODO for later: raise an error for the shape where data is of shape `space.shape`: tell the user to expand_dims the axis-0
  - 2. Log is not working, because numpy does not have automatic differentiation. Instead, use: `export GEOMSTATS_BACKEND=pytorch`
- Luis updates the group on the work with Anna: the goal is to finish the module `geodesic_metric_spaces`, including the Wald space. Interest in spaces of random matrices. LowerTriangularMatrices --> SymmetricStochasticMatrices. Collaborators may be interested in inexact computations, and investigating trade-off between exactness and computational times. Birkhoff polytope.
- Luis updates the group on the work with Alice: Luis & Alice have refactored the `discrete_curves.py` module.
- In SPDMatrices, most metrics are defined using a diffeomorphism: now we have a class `Diffeormorphism` (which impacts Malik's project).


Meeting: 2023/10/03
-------------------
Present: Xavier, Luis, Nina.
- Hackathon in Sophia-Antipolis, on-site, February or March. 10-20 people. Travel, accomodation not funded. Lunch provided. Dinner not provided.
- Information geometry (Luis):
  - Tests + codebase were refactored in information geometry -> PR next week.
  - The group discusses the limits of the Fisher-Rao metric with automatic differentiation. We should try to go beyond the basic, 1-dim examples.
  - Luis is talking with Frank Nielsen to implement more functionalities.
- Learning (Luis):
  - Tests refactored. There is a problem with tangent PCA, we need more tests, we need to define what is the exact output of the algorithm. We need to correct for the metric in the computation of the covariance matrix (multiply it with the metric/co-metric on both sides: we need to diagonalize the (1,1)-tensor, i.e. the endomorphism). Or use an orthogonal basis in the tangent space, then express the Logs in that basis and diagonalize the covariance matrix from these.
- Numerics in tests (Luis):
  - n_trials variable, x_fails marker in pytest: to show when tests fail sometimes, either due to numerics or potentially to actual bugs in the codebase.
- Luis is on-boarding Malik on the codebase related to landmarks, curves, and surfaces.
- Simon could start working on the computation of the Log. Speeding up when there is an embedding. We could do approximate logs while controlling what's left, getting inspiration from Morten's work on the sub-Riemannian geodesics. Example of reaching an irrational number on the flat torus. Use the solvers to explore ideas very fast.
- Collaboration with Materials Department (ideas from: CFD simulations, artery, LDDMM, template).

Meeting: 2023/08/08
-------------------
Present: Xavier, Nina.
- FiberBundle and RiemannianSubmersion discussions.
- Xavier gives updates about math in the mine and the upcoming GSI hackathon.



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
