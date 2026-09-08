# HigherLevelOfAbstraction

**Expectation, intervention, and the geometry of error**

Research notes, 8 September 2026. Written from Antti's question about efference copy, expectation versus error, matrices, and transformers. This is a conceptual handoff with analytical examples and proposed experiments; no new benchmark results are claimed.

The question inherited from the recent repos is:

> Which distinctions can a bounded system preserve and recover through its available actions, while accounting for uncertainty, memory cost, and the effects of asking?

Antti's next suggestion supplies a useful mechanism: make a prediction about the return from an action, compare it with the actual return, and use the discrepancy to decide what to investigate or change.

My proposed next abstraction is:

> A useful internal state preserves predictions about consequential experiments. An error challenges those predictions. Further interventions help determine which distinction the system needs to revise.

**The response repertoire becomes an expectation**

Let h be the observed history and q an experiment, potentially a sequence of actions. Write

$$
\mathcal R_h(q)=P(Y\mid h,\operatorname{do}(q)).
$$

This is the distribution of future observations if we perform q. The system has a learned approximation to it. A simple predictor summarizes that distribution with a mean and covariance:

$$
\hat y(h,q)=\mathbb E_{\hat{\mathcal R}_h(q)}[Y],
\qquad
r=y-\hat y(h,q).
$$

The residual r is the difference between the observed and predicted return. In state estimation this kind of new discrepancy is called an innovation.

An efference copy fits into this scheme as a copy of the outgoing command supplied to a prediction pathway. The command copy tells the predictor what was attempted; a learned forward model and relevant context predict the sensory consequences. The command copy, predicted return, and prediction error are different objects.

A prediction of the complete return and a prediction of the self-generated contribution are also different. A full expectation may include expected external events. A cancellation mechanism needs an explicit account of which contribution it is supposed to subtract.

The familiar additive idealization is

$$
y=y_{\rm self}+y_{\rm external}+\epsilon,
\qquad
r_{\rm self}=y-\hat y_{\rm self}
=y_{\rm external}+(y_{\rm self}-\hat y_{\rm self})+\epsilon.
$$

That equation explains both the appeal and the vulnerability. A good self-predictor makes external changes easier to detect. Its own errors also appear in the residual. For nonlinear interactions, an additive self/external split may itself be a bad model.

Electric-fish experiments provide an actual biological example of learned cancellation: command-related signals and sensory experience produce negative images of predictable sensory responses. Work on their temporal basis also shows why the timing of a prediction matters [1,2]. This supports the mechanism as biological inspiration; it does not establish that every dendrite or transformer implements it.

**An error is evidence of a mismatch, not a diagnosis**

When a return is surprising relative to a calibrated prediction, possibilities include a changed world, a changed sensor, a changed actuator, an omitted state, an inaccurate predictor, or ordinary noise.

Conversely, a familiar or expected external event can still matter and need remembering. Small error is not a certificate of truth or relevance. A predictor can become excellent on a narrow stream that the system's own actions keep producing.

For approximately Gaussian predictive uncertainty, a useful magnitude is

$$
S=r^\top\Sigma^{-1}r.
$$

Sigma must include uncertainty in the prediction and measurement, with their correlations handled appropriately. It is not automatically just sensor noise. For categorical or multimodal outcomes, a predictive likelihood or another proper probabilistic score is more suitable than subtracting numeric means.

This is how expectation versus error connects to measuring future behavior: make the prediction before the return arrives, score it on that later return, and check whether the discrepancy improves subsequent decisions. Re-explaining an answer after seeing it is a different exercise.

**Replacing the image with a matrix makes the bridge explicit**

A simple experiment can use

$$
y=CAq+\epsilon,\qquad \hat y=C\hat A q,
$$

where q is an allowed input, A is the hidden material or transformation, and C is the available readout. Then

$$
r=C(A-\hat A)q+\epsilon.
$$

A residual exposes the operator discrepancy along the direction excited by q and retained by C. It does not reveal the whole matrix. A difference D satisfying CDq=0 for every permitted q remains invisible through this interface.

If the material changed from a baseline A0, the residual contains both actual change and baseline prediction error:

$$
r=C\Delta A q+C(A_0-\hat A)q+\epsilon.
$$

This is exactly why calibration and the baseline resource matter in Active Dendrite. For dynamic measurements, replace CAq by the appropriate sequence of responses, such as CA^kBq.

Here is an analytical two-dimensional example, with delta > 0:

$$
A_\pm=
\begin{bmatrix}
1 & \pm\delta\\
0 & 1
\end{bmatrix},
\qquad
C=\begin{bmatrix}1&0\end{bmatrix},
\qquad
\hat A=I.
$$

| Probe | Predicted return | Actual return | What the probe establishes |
|---|---:|---:|---|
| q = (1,0) | 1 | 1 under both materials | No distinction between the materials |
| q = (0,1) | 0 | +delta or -delta | The same sensor can distinguish them if noise permits |

Both materials even have the same eigenvalues. The difference concerns a route from an input to a readout. This is an abstract signed matrix example, not a model of passive dendritic conductances.

An image is one possible state representation or display. The response geometry does not require it.

**The “angle of mistake” is a useful intuition with a specific meaning**

There are three distinct questions:

- How large is the discrepancy relative to uncertainty?
- Which proposed cause predicts its direction?
- Which change to the predictor would reduce future error?

A raw angle between the expected and actual vectors can miss important failures. The vectors (100,0) and (101,0) have zero angular difference, yet their discrepancy is nonzero. Angles involving a zero vector are undefined.

A more useful comparison is between the residual and the signature predicted by a candidate change.

For small changes in a nonlinear model, stack the measured samples and write

$$
r \approx J_{\rm target}\,\delta\theta
      +J_{\rm nuisance}\,\delta\nu+\epsilon.
$$

The columns of J describe how different parameter changes would alter those measurements. Compare directions after accounting for uncertainty. If L is an invertible whitening transform with L Sigma L^T = I, define

$$
\tilde r=Lr,\qquad
\tilde s_i=LJ_i,\qquad
\cos\phi_i=
\frac{\tilde r^\top\tilde s_i}
{\|\tilde r\|\,\|\tilde s_i\|}.
$$

A large alignment can support a candidate explanation, but magnitude, priors, and competing explanations still matter. For unknown signed change magnitude, compare against both directions or the relevant subspace. An angle is not a posterior probability.

A simple illustration is two residual signatures proportional to (1,1) and (1,-1). Their magnitudes can match while their directions differ completely. A single scalar error norm throws that distinction away.

At one sensor and one instant, however, there is only magnitude and sign. A richer angle requires multiple time samples, sensors, or trials. In latent representations, raw Euclidean angles also depend on scaling and coordinates; comparisons should use a declared metric.

The gradient is different again. With fixed Sigma and quadratic loss, a predictor-parameter gradient is

$$
\nabla_\psi \mathcal L
=-J_\psi^\top\Sigma^{-1}r.
$$

That tells us how the model could change to reduce the loss locally. It does not prove that the corresponding physical parameter caused the error. Observation space, parameter space, and action space must not be silently identified.

The experiment-design question becomes: which next probe makes the plausible explanations predict measurably different residuals?

**What this suggests for a neuron**

A distributed neuronal state could supply temporal response patterns. A command-related pathway could help predict the contribution of a known action. Local or downstream circuitry could compare parts of the observed response with that prediction.

The developmental hypothesis is that plasticity shapes which consequential distinctions reach the available readout. It may alter their timing, gain, nonlinear response, or resistance to nuisance.

This is a hypothesis about learning useful response geometry. It does not follow merely from a waveform resembling an ECG, a large residual, or an operator having a persistent mode.

There is an access constraint: one scalar soma residual does not automatically provide separate pre- and postsynaptic residuals at every branch. A proposed local plasticity rule must identify the signals each location actually receives. Predictor weights, temporary state, calibration history, and command copies all count as resources.

**What I see in transformers**

The established architectural connection is limited but useful. Standard attention computes

$$
\operatorname{Attention}(Q,K,V)
=
\operatorname{softmax}(QK^\top/\sqrt{d_k})V.
$$

Its routing weights depend on the input representations. A residual connection adds a sublayer output to its input; it is not, by definition, a measured prediction error. Likewise, an attention query is a vector used in compatibility calculations, not automatically an intervention on an external system [3].

For a frozen transformer block F and a small continuous input perturbation q, an experimenter can examine

$$
F(X+q)-F(X)\approx J_Xq.
$$

Unlike Sigh's fixed Fourier basis, this local sensitivity depends on X. Token replacements need finite-difference experiments; they are not automatically infinitesimal perturbations. A learned approximation to the block's response can be tested against actual returns.

That would measure whether the approximation captures the block's behavior. Agreement would not establish that the block's language output is factually correct.

My stronger proposed bridge is an explicit agent loop around a model:

| Function | Concrete implementation to investigate |
|---|---|
| Action record | Store the actual query, tool call, or other intervention |
| Expected return | Predict observable features of its result and their uncertainty before receiving it |
| Return comparison | Compare the result with that prediction |
| Investigation | Choose an additional action that separates plausible explanations |
| Memory update | Record warranted external evidence with its source and derivation history |

These functions could be learned using transformers. The architecture alone does not guarantee them.

A model rereading its own answer has received another representation of its earlier computation. That representation may help reasoning, but it is not an independent external witness. Source identity is especially useful here because software can record it explicitly. An efference-copy-inspired agent need not infer from correlation what its action log already identifies.

**One discrepancy should not automatically drive every learning process**

The same episode can support several different updates:

| Learning process | What evidence it needs |
|---|---|
| Echo or return predictor | Whether the predicted consequences matched the observed return |
| Query-selection policy | Whether the chosen action improved later prediction or decisions enough to justify its cost |
| Event memory | What external occurrence is supported, including whether apparently repeated evidence shares a source |
| Structural learning | Whether retaining a new distinction improves later behavior without unacceptable interference |

This is why no universal “make prediction fast” or “make prediction slow” rule follows. A stale self-model can corrupt a protected substrate; a predictor that absorbs a newly relevant signal too quickly can hide it from a surprise-only mechanism. Timescales have to be evaluated against the job each state performs.

Nor should all action-caused sensory effects be discarded. If pushing an object reveals its weight, the action's consequence is useful evidence. The question is which part is already explained, what remains uncertain, and which memory is authorized by that evidence.

**The additional jump: errors can challenge the abstraction itself**

Suppose two histories are assigned the same internal representation z. The system consequently predicts the same return for a given experiment.

If reproducible external returns show that these histories require different predictions or different decisions, the representation may need to separate them. Other options are to improve calibration, remember a missing context, or change the experiment. Error alone does not establish that another memory or expert must be created.

A proposed criterion is:

> Keep a new distinction when it improves predictions or decisions on later evidence enough to earn its storage and interrogation costs.

This connects the response repertoire to the question in 368 about whether an old model earns continued existence. It also connects to Jello's difficulty maintaining competing routes in shared material.

Learning can merge histories that are equivalent for the tasks and interventions that matter, while separating histories that lead to consequentially different futures. A distinction unnecessary today may become necessary after the task changes.

An expectation is therefore a testable claim about what the current abstraction has preserved. An error is a reason to investigate that claim. An intervention can help decide how to revise it.

**Where the earlier repos fit**

- [GeometricNeuronOriginReview](https://github.com/anttiluode/GeometricNeuronOriginReview), from [PerceptionLab](https://github.com/anttiluode/PerceptionLab): the checkerboard-to-measurement block is a deterministic scalar response function at fixed settings, with scheduling delays preserved. The controller history and feedback generate the rhythm. The first four feedback values were spatial samples, not eigenmodes.
- [SighImageSuper](https://github.com/anttiluode/SighImageSuper): distinguishes persistence, travelling state, structural memory, active readout, and the hazards of learned self-prediction.
- [OperaattoriAktiivinenDendriitti](https://github.com/anttiluode/OperaattoriAktiivinenDendriitti): chooses measurements that separate target changes from nuisance, with baseline uncertainty and cost.
- [Operaattori](https://github.com/anttiluode/Operaattori): supplies morphology-to-response calculations and geometry sensitivities.
- [JelloBrain](https://github.com/anttiluode/JelloBrain): tests how activity writes future routing and how competing writes interfere.
- [368](https://github.com/anttiluode/368): distinguishes adaptation, retention, retrieval, and investigation.

Predictive state representations already formalize representing state through predictions about experiments [4]. Dual control already treats actions as both influencing and investigating uncertain systems [5]. The proposed work here is to connect those ideas to counted memory, writable substrates, error attribution, and useful structural distinctions.

**A small future experiment, not work claimed in this note**

Start with a hidden matrix and a learned response predictor. Give the observer restricted probes and a restricted readout. Present three possibilities: unchanged material, changed material, and changed measurement gain.

Require predictions before returns arrive. Compare residual magnitude alone with candidate residual signatures, using fixed and adaptive probes under equal budgets. Include an exactly blind change and an unchanged control. Predictor parameters and any retained baselines count.

A held-out probe should check whether an inferred change predicts something beyond the observation used to fit it. If adaptive selection does not beat a strong fixed panel, preserve that outcome.

Only then add writable material and correlated external disturbances. Sigh's private-dither result already gives a warning: when outside effects respond to the same perturbation as the presumed self-response, cancellation becomes confounded.

The meaningful success criterion is improved future decisions with calibrated uncertainty, while remaining able to update after external change. Lower residual magnitude by itself is insufficient.

**Primary references**

1. Bell, C. C. (1981). *An efference copy which is modified by reafferent input.* Science 214, 450–453. [DOI](https://doi.org/10.1126/science.7291985).
2. Kennedy, A., et al. (2014). *A temporal basis for predicting the sensory consequences of motor commands in an electric fish.* Nature Neuroscience 17, 416–422. [DOI](https://doi.org/10.1038/nn.3650).
3. Vaswani, A., et al. (2017). *Attention Is All You Need.* [Original paper](https://proceedings.neurips.cc/paper/2017/file/3f5ee243547dee91fbd053c1c4a845aa-Paper.pdf).
4. Littman, M. L., Sutton, R. S., and Singh, S. (2001). *Predictive Representations of State.* [Original paper](https://proceedings.neurips.cc/paper/2001/file/1e4d36177d71bbb3558e43af9577d70e-Paper.pdf).
5. Klenske, E. D., and Hennig, P. (2016). *Dual Control for Approximate Bayesian Reinforcement Learning.* JMLR 17(127), 1–30. [Paper](https://www.jmlr.org/papers/v17/15-162.html).

The matrix examples and proposed connections above are analytical notes and research hypotheses. This repository does not yet demonstrate a biological observer, a transformer with reliable causal self-monitoring, or a new learning benchmark.
