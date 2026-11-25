# Vision-Language Models, Vision-Language-Action Models, and Physical AI: An Engineering Guide to the 2026 Landscape
Research status: August 20, 2026. Scope follows the requested deep-research brief, with particular emphasis on first principles, 2025–2026 developments, semiconductor implications, and Israel.
Executive overview
A useful mental model is that modern robotics foundation models are attempting to repeat, in the physical world, the transition that language AI made from narrow task-specific systems to broadly pretrained models. The critical difference is that physical mistakes have stateful, costly, and sometimes dangerous consequences: a wrong text token can be deleted, while a wrong robot action can break a glass, damage a motor, injure a person, or move the world into an unrecoverable state.
What is a VLM? A Vision-Language Model associates visual information with language. At the simplest end, CLIP learns a common embedding space in which related images and text lie close together. At the generative end, models such as Flamingo, BLIP-2, LLaVA, Qwen-VL and current Gemini/GPT-class multimodal models convert visual features into tokens or latent representations that a language model can use for captioning, visual question answering, OCR, grounding, scene interpretation and increasingly video/ spatial reasoning. CLIP's seminal formulation learned from paired web image-text data; later systems added cross-attention, modality bridges, instruction tuning, larger vision backbones and richer visual tokenization.
What is a VLA? A Vision-Language-Action model extends the same idea across the perception-action boundary. Instead of ending at text such as “the red screwdriver is left of the blue box,” it conditions on images, a language goal and usually robot state, then predicts executable actions: end-effector deltas, gripper commands, joint targets, trajectories, continuous action chunks or discrete action tokens. RT-1 established a Transformer-based language-conditioned closed-loop robot policy; RT-2 explicitly cast robotic actions as tokens in a VLM-like model; later systems such as OpenVLA, π0, GR00T and Gemini Robotics use substantially larger multimodal priors and richer action decoders.
The core difference is therefore: a VLM can describe what the robot should do; a VLA can generate control-relevant actions to do it. That does not mean every VLA is a complete robot. High-rate servo loops, force limits, emergency stops, collision checking, state estimation and sometimes IK, SLAM or motion planning often remain outside the learned model. Figure's Helix 02 is notable precisely because Figure claims a much more end-to-end extreme—vision, touch and proprioception in, all actuators out—whereas Google's 2026 Gemini Robotics architecture explicitly includes both high-level embodied-reasoning models and lower-level VLA execution models.
1
2
3
Why do VLAs matter for Physical AI? Classical robots excel where engineers can specify the geometry, objects, sequence, failure states and control logic in advance. A foundation policy potentially changes the marginal cost of adding a new task: rather than creating a new perception pipeline, state machine and motion-planning integration for every SKU or environment, one adapts a broadly pretrained model with demonstrations or instructions. Open X-Embodiment showed measurable positive transfer from pooling robot data across 22 embodiments; Octo showed that a compact generalist policy could be adapted with roughly 100 demonstrations in its reported experiments; DROID showed that adding diverse, in-the-wild data improved robustness and out-of-distribution performance.
The leading 2026 technologies are no longer just RT-2-era arm policies. Google DeepMind introduced Gemini Robotics 2 in July 2026 for whole-body humanoid and bimanual control, together with Gemini Robotics-ER 2 and an updated on-device model; Physical Intelligence now lists π0.7, released in April 2026, as a steerable robot foundation model with improved generalization; NVIDIA's GR00T 1.7, released in 2026, is a roughly 3B-parameter model trained with about 32,000 hours of real demonstrations plus about 8,000 hours of simulation according to NVIDIA; Figure is demonstrating its proprietary Helix 02 on multi-minute whole-body tasks; and the open ecosystem now includes π0-family models, GR00T, SmolVLA and many other policies through LeRobot.
How mature is it? Open-vocabulary perception is already highly useful; learned tabletop manipulation is laboratory-ready and in some constrained commercial settings early-commercial; narrow warehouse and factory humanoid tasks are entering real deployments; but robust, general household autonomy, reliable long-horizon reasoning, arbitrary tool use, cross-embodiment transfer and dexterous manipulation remain research or prototype capabilities. Figure reports industrial use at BMW and Agility has a commercial Digit agreement with Toyota Motor Manufacturing Canada, while Boston Dynamics says its first production Atlas deployments are scheduled for Hyundai and Google DeepMind in 2026. Those are important commercialization signals, but they are not evidence that general-purpose humanoid autonomy is production-ready.
The global leaders occupy different parts of the stack. Google DeepMind is exceptionally strong in robot foundation models, embodied reasoning and world models; NVIDIA has the broadest vertically integrated developer stack across training compute, CUDA, simulation, synthetic data, world models, GR00T and edge deployment; Physical Intelligence is a leading pure-play robot-foundation-model laboratory; Figure couples a proprietary VLA to a vertically integrated humanoid fleet; Hugging Face is rapidly making the open ecosystem accessible through LeRobot. Boston Dynamics, Agility and Apptronik combine high-quality robot hardware with increasing amounts of learned control and foundation-model partnerships.
Israel has meaningful ingredients but not yet a globally dominant robot-foundation-model cluster. Its strongest assets are computer vision, autonomous systems, AI research, semiconductor engineering, defense autonomy, Mobileye's embodied-autonomy experience and a dense startup ecosystem. The clearest recent Physical-AI move is Mobileye's 2026 acquisition of Israeli humanoid startup Mentee Robotics for total consideration of approximately $900 million. Enigma emerged in 2026 around robot-agnostic foundation models; Pickommerce is commercializing AI-driven logistics manipulation; Israel's Ministry of Defense has a dedicated AI and Autonomy organization; and the national AI infrastructure program has opened access to an infrastructure allocation totaling 1,000 NVIDIA B200 accelerators over coming years.
4
5
6
7
8
Israel does not appear, from the public national programs reviewed, to have a named, civilian “Physical AI Strategy” comparable to NVIDIA's framing or to a dedicated national humanoid program. That is an inference from the public program portfolio, not proof that no classified or unpublished plan exists. The ingredients for one already exist: national compute, AI research funding, robotics labs, defense autonomy, Mobileye/Mentee, manufacturing automation and semiconductor expertise.
The largest bottleneck is data, not merely model architecture. Internet text and images are cheap to copy and standardize; robot trajectories require physical hardware, operators, safety procedures, calibration and repeated interaction with the real world. Open X pooled more than one million trajectories across many institutions, DROID collected 76,000 trajectories over 350 hours using a standardized platform across 13 institutions, while NVIDIA says GR00T 1.7 now draws on tens of thousands of hours of real and simulated experience. Human egocentric video, simulation, synthetic trajectories and world models are being pursued largely because scaling only with teleoperated robot hours is economically difficult.
What should an experienced engineer learn first? First learn the distinction between representation learning and policy learning. Then understand coordinate frames, kinematics, control and robot observations. Run a VLM locally. Train ACT or Diffusion Policy on a small manipulation task. Only after that move to OpenVLA/π0/GR00T-class systems. Without that progression, VLAs can appear almost magical; with it, they become recognizable as large conditional policies sitting inside a closed-loop control system.
A compact progression is:
Each transition adds something important: sensory grounding → executable action → explicit prediction of consequences → longer-lived autonomous agency. Current systems have made major progressthrough the VLA stage; the final two stages remain incomplete.
Foundations: from language to embodied action
A rigorous glossary
Term Engineering meaning Example
LLM A large neural sequence model, usually Transformer-based, pretrained predominantly to model language token sequences.
“Explain how to grasp a mug.”
VLM
A model jointly processing visual and linguistic information. It may output embeddings, classifications, coordinates or language. CLIP is an embedding VLM; Flamingo/LLaVA-like systems are generative VLMs.
See a workbench and answer “Which wrench is largest?”
9
10
LLM text→ text
Multimodal LLM text + image/audio/video
→ text/media
VLM vision + language→
grounded representation / text
VLA vision + language + robot
state→ actions
World model + VLA predict futures + choose
actions
General embodied agent perceive + remember +
plan + act + learn
11
Term Engineering meaning Example
VLA
A multimodal model whose outputs include robot actions rather than stopping at a semantic description. RT-2 made the formulation explicit by encoding robot actions in the same token-like output space used by a VLM.
“Pick up the wrench” → Cartesian/gripper actions.
Multimodal foundation model
A broadly pretrained model spanning multiple modalities and reusable across many downstream tasks.
Text + image + audio + video Gemini family.
Embodied AI AI studied as an agent embedded in an environment, where sensing and action affect future observations.
A mobile manipulator exploring a kitchen.
Physical AI
Broader industry term for AI that reasons about and acts in the physical world, covering robots, autonomous vehicles, machines, simulators and world models. NVIDIA's current stack explicitly uses this framing.
Humanoid, autonomous forklift, self-driving system.
Robot foundation model
A broadly pretrained robot model intended to transfer across tasks, scenes and sometimes embodiments.
π0/π0.7, GR00T, Gemini Robotics.
World model A learned model of how a world evolves, often approximating , sometimes in pixels and sometimes in latent state.
Predict whether a cup will tip after a push.
Vision encoder Neural network turning pixels into compact features/ tokens. ViTs divide an image into patches and process them with self-attention.
RGB → 576 visual tokens.
Language encoder
Maps text to contextual features. Common in contrastive or encoder-decoder architectures.
“red screwdriver” → semantic vectors.
Language decoder
Autoregressively produces text tokens conditioned on context.
Visual tokens + question → answer.
Action head / action model
Converts shared multimodal features into an action distribution or trajectory. It can be autoregressive, diffusion-based, flow-matching based or a conventional regression head.
Latent → 16 future Cartesian actions.
Policy model A model of behavior: . Observation + goal → next action.
Robot policy A policy whose observations/actions correspond to a robot's sensors and actuators.
Camera + joints → arm command.
12
13
14
15
p(s ∣t+1:T s , a )t t:T
π(a ∣t o , g)≤t
Term Engineering meaning Example
End-to-end robotic policy
A learned mapping spanning much of the raw sensor-to-action path rather than manually decomposing perception, planning and action. “End-to-end” need not mean that motor electronics or safety loops disappear. Figure describes Helix 02 as an all-sensors-to-all-actuators system.
Pixels → whole-body actuator targets.
Imitation learning
The general problem of learning behavior from demonstrations.
Learn grasping from teleoperated episodes.
Behavior cloning Supervised imitation learning: directly fit demonstrated actions from observations/goals.
Minimize error between model and demonstrator actions.
Reinforcement learning
Learn a policy from reward/return rather than only reproducing labeled demonstrations.
Reward successful insertion, penalize collisions.
Inverse dynamics
Infer the action that explains a state transition: .
Infer hand motion from before/after video.
Forward dynamics
Predict the next state from present state and action. Predict where the cup moves if pushed.
Affordance An action possibility grounded in object/environment geometry and semantics.
A handle affords grasping/pulling.
Embodied reasoning
Reasoning whose conclusions depend on physical geometry, action possibilities, constraints and task context. Google's Robotics-ER family is explicitly designed around this role.
“I need two hands, so put the bottle down first.”
Spatial reasoning
Understanding position, geometry, orientation, topology and relations.
“The screwdriver is behind the box.”
Temporal reasoning
Understanding event order, persistence and evolution. “The drawer must be opened before reaching inside.”
Action token
A discrete symbol encoding an action or part of an action. RT-1 quantized dimensions; RT-2 reused tokenlike output machinery; OpenVLA originally uses discretized action bins.
Token 173 → +3 mm x motion in a normalized bin.
Continuous action space
Actions remain real-valued vectors. Flow/diffusion policies can model these directly.
.
Discrete action space
Continuous values are quantized or the robot chooses among symbolic primitives.
{left, right, grasp, stop}.
16
a ≈t f(s , s )t t+1
17
18
[dx, dy, dz, dR, g]
Term Engineering meaning Example
Robot state / proprioception
Internal body measurements: joint angles, joint velocities, motor current/torque, base state, endeffector pose, gripper state and similar quantities.
, gripper opening.
A particularly important distinction is that “VLA,” “reinforcement learning” and “diffusion policy” refer to different axes of the design space. VLA describes what modalities condition the policy and what it outputs. Reinforcement learning describes a learning signal. Diffusion describes a way of parameterizing/ generating an action distribution. A VLA may therefore be pretrained by behavior cloning, use a diffusion or flow-matching action head, and then undergo RL post-training. Physical Intelligence's 2026 research program explicitly includes online RL improvements to its VLA family, while Diffusion Policy's original formulation was evaluated largely as behavior cloning.
What changes at each level
Class Typical input
Typical output Internal representation
Can do Cannot by itself reliably do
LLM Text Text/tokens Linguistic token embeddings
Knowledge, planning descriptions, code
Directly perceive current physical scene
Multimodal LLM
Text + images/ audio/video
Usually text/ media
Shared multimodal token context
Interpret multiple modalities
Direct robot control unless explicitly actiontrained
VLM Images/ video + text
Text, boxes, points, embeddings
Vision-language features
Recognition, grounding, OCR, VQA, scene reasoning
Turn semantic understanding into reliable motor trajectories
VLA Vision + language + often state
Robot actions/ chunks
Shared perceptual/ task/action latent
Direct closedloop control
Guarantee safety, perfect physics or longhorizon success
World model + policy
State/vision + candidate actions
Future states + selected actions
Predictive dynamics latent
“Imagine” alternatives before acting
Guarantee its imagined dynamics match reality
General embodied agent
Multimodal history, goals, memory
Plans, actions, communication
Long-term memory + world/policy state
Persistent autonomy
Still an unsolved research target
q ,… , q1 7
19
A VLM “reasoning about an image” should not be anthropomorphized. It is performing learned computation over representations that can support impressive inference, but the computation is not guaranteed to respect Euclidean geometry, object permanence or Newtonian mechanics. Gemini's own current model cards continue to list hallucination as a general limitation of foundation models.
How a modern VLM works
A canonical pipeline is:
A Vision Transformer first projects image patches into vectors and uses attention to contextualize them. What happens next distinguishes architectures.
CLIP trains independent image and text encoders so matched image-text pairs have similar embeddings and unmatched pairs separate. That yields remarkably transferable semantic representations but is not, by itself, a conversational generator.
Flamingo takes frozen visual and language backbones and inserts learned components that allow the language model to attend to interleaved images, video and text. DeepMind's largest reported Flamingo used a 70B Chinchilla-based language model as part of an approximately 80B-parameter system.
BLIP-2 makes the bridge explicit through a lightweight Q-Former, keeping both the image encoder and LLM frozen during much of training. This architecture helped establish the now-common pattern “good visual encoder + small projector/adapter + good LLM.”
LLaVA showed how surprisingly far a simple visual projector plus visual instruction tuning could go, helping create the modern open multimodal-model ecosystem. Later LLaVA-NeXT variants increased visual resolution and extended into multi-image, video and multi-view/3D settings.
PaLI instead used a ViT together with a text Transformer encoder and Transformer decoder, training across many vision-and-language tasks and more than 100 languages.
Qwen2.5-VL illustrates another 2025-era design direction: native dynamic-resolution visual processing, grounding through points and boxes, and ViT architectural choices optimized for multimodal workloads. Alibaba released 3B, 7B and 72B variants, making the smaller two especially practical for local experimentation.
As of 2026, commercial frontier models increasingly blur the line between “VLM” and “multimodal LLM.” Google's Gemini 3.x family is natively multimodal across text, image, audio and video, and OpenAI's current smaller GPT-5.4 variants advertise multimodal understanding and screenshot interpretation. These systems are commercially deployed, but their weights are not available for local robotics experiments.
Representative VLM landscape
20
pixels → patches → vision features → modality bridge → LLM-compatible tokens → attention → text/grounding output
21
22
23
24
25
26
27
Model/family Main architectural idea Status Local use
CLIP Contrastive dual encoders Foundational research; open code/model lineage
Excellent
Flamingo Frozen vision/LLM + gated cross-attention/resampling
Research milestone Original large model not a normal local option
BLIP/BLIP-2 Caption/contrastive pretraining; Q-Former bridge
Open research Good
LLaVA Vision encoder + lightweight LLM projector + visual instruction tuning
Open research ecosystem
Good at smaller sizes
PaLI ViT + text encoder-decoder multitask training
Google research Limited availability
Qwen-VL family Dynamic-resolution ViT, grounding and multimodal instruction tuning
Open-weight + commercial
3B/7B realistic locally
InternVL Large open multimodalmodel research family
Open-weight research Smaller/quantized variants practical
Gemini 3.x Native multimodal frontier model family
Commercial API/cloud rather than local
GPT multimodal family
Unified multimodal reasoning/generation family
Commercial API/cloud; weights closed
NVIDIA Cosmos Reason/related models
Physical-world multimodal reasoning and world-model stack
Open/developer physical-AI stack
Smaller variants edge/datacenter dependent
The key insight is that visual tokens need not literally “look like words.” They are vectors. The model only needs a learned interface that places enough visual information into a representation the shared Transformer can use. Likewise, action tokens do not have to resemble language semantically: token 537 can simply mean a normalized motion bin. The Transformer machinery is agnostic to whether token IDs ultimately represent words, image codes or robot commands.
From VLM to VLA
The simplest abstraction is:
26
28
29
30
I , L, s( t t)⟶ πθ
at:t+H
where is one or more camera observations, is the instruction, is proprioceptive state, and is either one action or a chunk of future actions.
Those actions may be:
Representation Example Benefits Weaknesses
Joint position Direct mapping to position servos
Robot-specific morphology
Joint velocity Reactive Sensitive to timing
Torque Maximum low-level authority
Safety/control difficulty; high frequency
End-effector pose More transferable across arms
Requires IK/controller underneath
Gripper command open/close or width Simple Hand dexterity not captured
Base command Useful mobile robot abstraction
Separate navigation/ stability issues
Action trajectory Temporal coherence Open-loop error if chunk too long
Discrete tokens Quantized action dimensions
Fits autoregressive LLM machinery
Quantization + token latency
Continuous diffusion/flow
Real-valued action chunk
High-dimensional/ multimodal outputs
Repeated denoising or flow compute
RT-1 discretized its actions and ran closed-loop around 3 Hz. ACT directly predicted joint-position action chunks at 50 Hz in its reported ALOHA experiments. OpenVLA's maintainers recommend roughly 5–10 Hz data/control for the original single-step architecture, while π0 was designed to produce smooth continuous high-frequency action trajectories with flow matching and can operate at up to tens of hertz. This enormous spread is why “VLA latency” cannot be discussed without specifying the action abstraction.
Action chunking predicts future controls at once. The model therefore spends one expensive inference call producing perhaps 10–100 low-level targets. ACT helped popularize the idea; FAST later compresses action sequences into tokens more efficiently, and newer runtimes can asynchronously calculate the next chunk while the current one executes. OpenVLA's repository reports up to 15× token-side acceleration for FAST-style action tokenization and 25–50× faster inference for its later optimized fine-tuning recipe relative to vanilla OpenVLA. These are project-reported improvements, not universal hardware-independent speedups.
Closed loop versus open loop is equally important. An action chunk may be generated open-loop, but the overall system can still be closed-loop by observing again after a short prefix of that chunk. In receding-
It L st at:t+H
q1:7 \*
q̇1:7
τ1:7
Δx, Δy, Δz, ΔR
v,ω
at:t+H
31
H
32
horizon operation the robot repeatedly executes a limited section, re-observes and replans. Diffusion Policy explicitly incorporates receding-horizon control for this reason.
Does VLA replace the classical robotics stack? In 2026, the practical answer is partially, and at different rates by subsystem:
Classical component Learned replacement pressure
Likely 2026–2030 outcome
Object classification / semantic perception
Very high Largely foundation-model based
Hand-coded task state machines
Very high Increasingly replaced by learned/high-level agent policies
Grasp-point selection High Increasingly learned
Local manipulation trajectory generation
High VLA/diffusion increasingly competitive
Global navigation / localization / SLAM
Medium Hybrid; geometric methods remain valuable
IK Medium-low Often retained under Cartesian learned policy
Collision checking Low-medium Learned proposals + classical verification likely
Hard safety envelopes Low Classical/formal mechanisms remain critical
Inner motor-current/torque servo
Very low Deterministic real-time control persists
Emergency stop / functional safety
Very low Must remain independent of generative uncertainty
The most plausible architecture is therefore not “Transformer instead of robotics,” but foundation model around a hard real-time robotic kernel.
Models, training, robot data, and world models
The historical progression is easier to understand as a sequence of attempts to increase task diversity, data diversity, embodiment diversity and action expressiveness.
33
System Release Organization
Core architecture / action representation
Training/data Public? What was actually demonstrated
RT-1 2022 Google
EfficientNet/ FiLM visual features → TokenLearner → Transformer → discretized action tokens
>130k episodes, >700 tasks, 13 robots over 17 months
Model/data released
Closed-loop languageconditioned manipulation; reported 97% seen-task and 76% unseeninstruction performance in its evaluation.
ACT / ALOHA 2023 Stanford
Conditional-VAE Transformer predicts jointposition chunks
~50 demos/ task in flagship experiments
Code/hardware open
High-frequency bimanual tasks such as battery insertion and shoe handling.
Diffusion Policy
2023 Columbia/ TRI/MIT
Conditional diffusion over action sequences
Robot demonstrations across simulated/real benchmarks
Code/data
Strong behavior cloning over multimodal, highdimensional action distributions; extended study reports 46.9% average improvement over compared methods.
34
35
33
System Release Organization
Core architecture / action representation
Training/data Public? What was actually demonstrated
RT-2 2023 Google DeepMind
PaLI-X/PaLM-E VLM cofinetuned on web + robot data; actions represented as tokens
Web-scale visionlanguage + robot trajectories
Model not broadly open
Better semantic generalization than RT-1; Google reported unseenscenario improvement from 32% to 62% in one evaluation.
RoboCat 2023 DeepMind
Goal-conditioned generalist Transformer/ decision model
Multiple tasks and embodiments; self-generated robot data
Research
Adaptation to new tasks/ robots with roughly 100– 1,000 demonstrations plus self-improvement loop.
RT-X / Open X-Embodiment
2023– 24
34-lab collaboration
Common action representation + RT-X models
>1M trajectories, 22 embodiments, 60 pooled datasets
Dataset/model components open
Positive transfer across robots and tasks; RT-2-X reported stronger emergent skills than singledataset baselines.
Octo 2024
Berkeley/ Stanford/ CMU/ DeepMind
Transformer-based diffusion policy, 27M/ 93M
~800k OXE episodes, 25 datasets
Code/weights
Adaptation across observations, action spaces and embodiments; evaluated on nine real robot setups.
12
36
37
38
System Release Organization
Core architecture / action representation
Training/data Public? What was actually demonstrated
OpenVLA 2024 Stanford/ Berkeley/TRI collaborators
~7B-class VLM transformed into autoregressive discrete-action policy
Open X mixture including BridgeData
Code/weights/ data recipes
First widely accessible large VLA; LoRA and full finetuning recipes available.
π0 2024 Physical Intelligence
~3B VLM backbone + continuous flow-matching action expert
Internet VLM data + OXE + proprietary dexterous robot data on eight robot types
Open implementation/ checkpoints later released
Complex dexterous tasks and multiembodiment generalization; PI explicitly described it as an early step, not solved general robotics.
SmolVLA 2025 Hugging Face
Compact 450M VLA, asynchronous inference
Open LeRobot community datasets
Fully open
Consumer-hardware-oriented VLA; HF reports strong LIBERO/ Meta-World and SO-100/101 results.
GR00T N1 → N1.5 → N1.6
2025 NVIDIA
VLM “slow” system + continuous/ actiondiffusion “fast” system; later crossattention designs
Human video, real/sim robot data, synthetic data
Open weights/ tooling
General humanoid policy experiments across several embodiments.
32
39
40
41
System Release Organization
Core architecture / action representation
Training/data Public? What was actually demonstrated
Gemini Robotics 1.x
2025– 26
Google DeepMind
Gemini-derived embodiedreasoning model + VLA variants
Proprietary multi-robot data
Limited access
Dexterous manipulation, transfer and embodied planning; separate ER and motor-control roles.
Helix / Helix 02
2025– 26
Figure
Proprietary humanoid VLA; Helix 02 claims unified full-body visuomotor network
Proprietary humanoid fleet data + human video initiatives
Closed
Figure shows multi-minute kitchen/living-room tasks and BMW workflows. Treat performance assertions as company demonstrations.
π0.7 2026 Physical Intelligence
Successor in π0 family; steerable foundationpolicy design
Proprietary + broad robot data; full scale not publicly enumerated in the source reviewed
PI openresearch ecosystem
PI describes a “step-change” in generalization and emergent behavior; independent replication remains important.
GR00T 1.7 2026 NVIDIA
~3B model; Cosmos-Reason/ Qwen3-VL-derived backbone + action model
NVIDIA reports ~32k h real + egocentric and ~8k h simulation
Weights available
Broader reasoning and manipulation; ONNX/TensorRT deployment.
42
43
44
45
System Release Organization
Core architecture / action representation
Training/data Public? What was actually demonstrated
Gemini Robotics 2
Jul. 2026
Google DeepMind
VLA + Robotics-ER 2 + on-device component in a three-model ecosystem
Proprietary Controlled access
Full-body humanoid feet-to-fingertips, dexterity and multi-robot scenarios.
The table exposes two distinct trajectories. One scales model knowledge by starting from a VLM and injecting robot data—RT-2, OpenVLA and π0. The other scales action modeling—ACT, Diffusion Policy, flow matching, FAST and modern action DiTs. Current leading systems combine both.
What one VLA training example actually looks like
A dataset episode can be represented conceptually as:
episode_id: kitchen_004918 
instruction: "open the upper drawer and put the spoon inside" 
t = 12.400 s 
  external_rgb: H x W x 3 
  wrist_rgb:    H x W x 3 
  joint_pos:    [q1 ... q7] 
  joint_vel:    [dq1 ... dq7] 
  ee_pose:      [x y z qx qy qz qw] 
  gripper:      0.034 m 
  action:       [dx dy dz dRx dRy dRz gripper_cmd] 
  success/reward: optional 
  calibration: camera intrinsics/extrinsics 
DROID's released schema illustrates the real form: synchronized external and wrist camera observations, language instruction and action streams; its raw dataset also includes proprioceptive trajectories and calibrated stereo data.
A behavior-cloning VLA optimizes something analogous to:
for tokenized actions, or a regression/diffusion/flow objective for continuous trajectories. Millions of correlated examples teach the network statistical regularities such as “when the mug moves left in camera space and my wrist joint changes this way, these motor commands tend to lead toward a successful grasp.”
46
47
θ =\* arg E [− log p (a ∣ θ min (o,l,a)∼D θ o, l)]
The robot is not learning a symbolic control law written by an engineer. It is learning an enormous conditional mapping over demonstrations.
VLM training
Modern VLM development usually combines several objectives rather than a single training procedure.
Contrastive learning learns global semantic correspondence—CLIP's image-text discrimination is the canonical example.
Image-caption and autoregressive training teach visual features to support language generation.
Multimodal instruction tuning presents dialogues such as image + “Where is the wrench?” + target response. LLaVA's success helped establish this as a key stage.
Grounding datasets add points, boxes, segmentation or region descriptions. Qwen2.5-VL, for example, explicitly supports box and point outputs.
Video training supplies temporal information and helps bridge static recognition toward motion understanding. LLaVA-NeXT's interleaved framework uses multiple frames for video and multiple views for spatial/3D tasks.
Synthetic data can be produced by stronger models, simulators, renderers or captioners, reducing manual labeling requirements.
VLA training
The principal data channels are more physical:
Teleoperation has a human move the robot through a leader arm, VR controller, SpaceMouse, joystick or tracked hand interface. DROID, for example, standardized a Franka-based data collection platform and VR teleoperation across institutions.
Kinesthetic teaching physically guides a compliant arm. It is intuitive but hard to scale.
Imitation/behavior cloning trains directly on those trajectories.
Offline RL adds rewards/value estimation without requiring fresh robot interaction.
Online RL lets the robot improve through active experience, potentially overcoming behavior cloning's inability to recover from states the demonstrator rarely entered. Physical Intelligence's March 2026 work explicitly explores extracting an RL-oriented representation from VLA models for efficient online improvement.
Simulation can produce far more trials than physical hardware, although sim-to-real mismatch remains substantial for contact, friction, flexible objects, sensors and actuator dynamics.
21
48
26
49
50
44
Domain randomization intentionally varies textures, lighting, camera poses, masses, friction and disturbances so the learned policy cannot overfit to one simulator configuration.
Real-to-sim builds simulation/digital-twin scenes from real observations, while sim-to-real transfers learned behavior back into physical robots.
The robot-data bottleneck
The difference from LLM training is profound. An LLM token does not require a $10,000–$100,000 mechanism to move through the world before it exists. Robot data does.
Open X-Embodiment was therefore a landmark not merely because it was large by robotics standards, but because it demonstrated the organizational feasibility of pooling more than one million trajectories from 60 datasets and 22 embodiments contributed by dozens of laboratories.
DROID attacked a different dimension: environmental diversity. Its 76,000 trajectories/350 hours covered 564 scenes and 86 tasks, collected by 50 operators across 13 institutions using essentially the same portable Franka platform. Co-training with DROID produced a reported 22-point absolute in-distribution and 17-point out-of-distribution improvement over its next-best comparison in the project's aggregate evaluation.
Octo then asked whether this heterogeneity could be turned directly into a reusable policy. Its 27M- and 93M-parameter models were trained on approximately 800,000 episodes from 25 Open X datasets and adapted to new setups with relatively small target datasets.
The emerging robot-data flywheel is therefore:
This explains why robot companies increasingly discuss manufacturing scale and data scale in the same sentence. Figure says its growing Figure 03 fleet increases the data streams available for autonomy research; Apptronik opened Robot Park facilities specifically to collect humanoid learning data; NVIDIA mixes real demonstrations, human egocentric video and simulation in GR00T. These are company-reported strategies, but the economic logic is compelling.
Can human video substitute for robot trajectories? It can help enormously with semantics, object interaction, task sequencing and latent action discovery, but there is a fundamental embodiment gap: a human wrist, fingers, compliance and reachability differ from a robot's. Figure claims zero-shot navigation transfer after training on egocentric human video, while NVIDIA explicitly incorporates egocentric video in GR00T. DeepMind's Genie shows a more general idea: an 11B-parameter model learned a latent action space from unlabeled internet video without ground-truth action labels. These results make human video a
37
51
38
Deploy robot fleet Collect trajectories + failures
Label / auto-caption / score
Train foundation policy Deploy better policy More tasks become automatable
Fleet grows + data diversity growsBuild digital twins /
simulators Generate synthetic edge
cases
52
serious data source, but they do not prove that videos alone can replace embodiment-specific motor experience for precise contact manipulation.
World models
A VLA asks:
A world model asks:
For the intuitive cup example:
VLA: “Push with cm/s in the x direction.”
World model: “If the gripper contacts the left side here, the cup probably translates right; if contact is above its center of mass, it may tip.”
A planner can then evaluate candidate futures and hand the selected subgoal or action to a policy:
The distinction is not absolute. A sufficiently large VLA may have implicit predictive dynamics embedded in its hidden representation without exposing an explicit future-state model. Conversely, an explicit generative world model may be used only for data generation rather than online planning.
DeepMind's Genie is a strong conceptual example: a spatiotemporal tokenizer, latent-action model and autoregressive dynamics model learned controllable interactive environments from unlabeled internet video.
NVIDIA's Cosmos program explicitly connects generative world modeling to Physical AI. By 2026 NVIDIA described Cosmos 3 as combining multimodal reasoning, world generation and action prediction across text, image, video, sound and action modalities. Its DreamGen research explores using generated video trajectories to teach robot behaviors. Both should be interpreted as research/developer technologies rather than proof of a universal, physically correct simulator.
Generative video is relevant because predicting plausible future pixels requires learning regularities of motion and object interaction. But visual plausibility is weaker than physical correctness. A video generator can create a convincing falling cup while getting momentum, friction, hidden contact forces or grasp stability wrong. For high-consequence planning, the future world model will likely combine learned priors with geometry, state estimation, system identification and explicit constraints.
53
“Given what I see and what I was asked, what action should I issue?”
“Given this world and this candidate action, what is likely to happen next?”
+4
s →t { , ,…} →ŝt+1:T (a )1 ŝt+1:T
(a )2 score futures → a →\* π
54
55
Engineering the stack: experiments, compute, and silicon
For an engineer, the fastest route to understanding is to build systems in increasing order of control authority.
Practical learning experiments
Level Build Suggested software
Hardware Planning cost class
What you learn
VLM
Camera → local VLM → scene Q&A/ grounding
PyTorch/ Transformers, Qwen2.5-VL or compact LLaVA-family model
Existing PC + webcam; ideally 8–16 GB+ GPU for a small quantized VLM
~$30–$100 camera if PC exists; GPU-dependent
Visual tokenization, grounding, latency
VLM + classical robot
Camera → VLM/ grounder → target pose → MoveIt → arm
ROS 2, MoveIt, Python, local/ API VLM
Webcam/ depth camera + SO-101/ WidowX/ other small arm
Roughly hundreds to a few thousand dollars depending arm/camera
Separation of semantic perception from deterministic control
Learned policy
Teleoperate → record data → ACT/ Diffusion/ SmolVLA → execute
LeRobot, ACT, Diffusion Policy
Low-cost leader/ follower arm + one or more cameras
Low-thousands-or-less hobby/lab class
Dataset quality, BC, action chunking
Foundation VLA
Fine-tune π0/ OpenVLA/ GR00T-class model
LeRobot, openpi, OpenVLA
24–80 GB GPU depending method
Workstation/ lab compute
Transfer learning, action representation, embodiment adaptation
The dollar bands are engineering planning estimates, not August 2026 retailer quotes; local taxes, servo selection and preassembled versus DIY hardware can move the number by several-fold.
For the first VLM experiment, Qwen2.5-VL's 3B/7B open models are a particularly useful starting point because they combine practical model size with OCR and visual grounding.
For the second level, use the VLM for what it is good at—“which object satisfies the natural-language instruction?”—but give geometric execution to a conventional stack. A depth camera or calibrated stereo pair estimates the target in 3D; MoveIt handles collision-aware kinematics and trajectory generation; the
26
robot controller closes the fast servo loop. This isolates multimodal-model errors from motor control and is probably the best architecture for a first Physical-AI prototype.
RealSense remains relevant as a robotics depth-camera platform, but one Intel-specific correction matters: RealSense completed its spinout from Intel in July 2025 and is now an independent company; Intel Capital participated in its $50 million financing. It should therefore no longer be treated as an Intel product division.
For the third level, Hugging Face's LeRobot is now unusually comprehensive. Its 2026 repository exposes standardized robot interfaces and datasets and implementations spanning ACT, Diffusion Policy, π0-family VLAs, GR00T N1.7, SmolVLA and world-model work. It supports affordable SO-series arms as well as larger systems.
For OpenVLA specifically, the project's documented practical requirements are revealing: LoRA fine-tuning of its 7B model can run with roughly 27 GB VRAM with adjusted batching, whereas full fine-tuning is described using a node of eight A100 GPUs.
Physical Intelligence's openpi documentation gives a modern comparison point: inference requires more than about 8 GB of GPU memory, LoRA fine-tuning more than roughly 22.5 GB, and full fine-tuning more than roughly 70 GB; the maintainers report testing on hardware including RTX 4090, A100 and H100 systems.
That means an enthusiast can realistically run a serious VLA and fine-tune smaller policies, while pretraining the current frontier from scratch remains an institutional project.
Compute orders of magnitude
Workload Practical 2026 compute class Primary bottleneck
Small 3B-class VLM inference
Single consumer/workstation GPU; quantization enables smaller memory footprints
VRAM, image-token count
7B VLM/VLA inference
~12–24 GB class is comfortable for many quantized/BF16 scenarios; architecture dependent
VRAM + latency
30B–70B VLM inference
High-memory GPU or multi-GPU; aggressive quantization possible
Memory capacity/ bandwidth
ACT/Diffusion Policy training
One good workstation GPU for modest datasets Dataset I/O + GPU
SmolVLA training/ fine-tune
Consumer/workstation-oriented GPU memory/data
OpenVLA LoRA ~27 GB documented minimum-class configuration GPU memory
π0-family LoRA >22.5 GB documented openpi reference GPU memory
π0 full fine-tune >70 GB documented reference Accelerator memory
56
57
32
58
32
58
58
Workload Practical 2026 compute class Primary bottleneck
Robot foundation pretraining
Multi-node datacenter-class accelerator clusters Compute + data + interconnect + storage
The last line is qualitatively different from inference. Training repeatedly performs forward and backward passes over very large datasets and stores optimizer state and activations. A deployed robot usually executes only a forward policy pass and can exploit quantization, caching, distillation and specialized accelerators.
The semiconductor view
A physical-agent computer is not just “an AI accelerator attached to motors.” It is a heterogeneous real-time system.
The important kernels include:
Workload Hardware characteristic that matters
ViT image encoder Dense matrix multiply, attention, image bandwidth
LLM/VLM decoder GEMM, attention, KV-cache bandwidth/capacity
Diffusion/DiT action head Repeated Transformer passes; tensor throughput
Flow-matching action model Dense neural inference over action chunks
Image preprocessing ISP/video codecs, CPU/SIMD/GPU
Depth/stereo Vision accelerator/GPU, memory bandwidth
SLAM/geometric estimation CPU/GPU mixed precision, low latency
Motor control Deterministic CPU/MCU/FPGA real-time execution
Sensor fusion CPU/vector/DSP/NPU
Fleet communications Ethernet/Wi-Fi/5G, security, deterministic networking
Training Accelerator compute + HBM + high-bandwidth fabric
Simulation GPU graphics + physics + AI compute
This leads to a crucial architectural point: the best chip for training a VLA is not necessarily the best chip to put inside a robot.
Training prioritizes aggregate tensor FLOPS, HBM, accelerator scale-out and network bandwidth. A robot prioritizes performance per watt, memory footprint, predictable latency, sensor I/O, thermal limits, functional safety and enough local compute to keep operating when connectivity disappears.
The likely robot architecture is therefore hierarchical:
Pure cloud control is unattractive for safety-critical fine manipulation because network latency and availability are external variables. Cloud/datacenter compute is extremely useful for training, fleet analysis and slow deliberation; the fast sensor-action loop belongs on-device.
NVIDIA's Physical AI stack
NVIDIA's strategic advantage is less any single model than the fact that it offers a plausible product at nearly every stage:
CUDA provides the underlying accelerated-compute ecosystem. DGX-class systems train large models. Omniverse provides the 3D/simulation substrate. Isaac Sim models sensors and robots; Isaac Lab provides robot-learning workflows. Cosmos provides world-model/reasoning/generation components. GR00T provides a humanoid/robot foundation-policy family. Jetson/Thor-class edge systems host deployment. NVIDIA's 2025–2026 disclosures show this stack being integrated by robot makers including Boston Dynamics, Agility, Fourier and others.
My maturity assessment of those layers is:
models / logs
Datacenter / fleet cloud training, replay,
world-model generation
Central robot SoC VLM/VLA, planning,
perception
Real-time CPU / safety computer
state estimation, limits, supervision
Distributed motor controllers
current / torque / position loops
Cameras, IMU, tactile, depth
Actuators
CUDA/DGX → Omniverse + Isaac Sim/Lab → synthetic data / Cosmos → GR00T → Jetson/Thor → robot
59
NVIDIA layer 2026 maturity Assessment
CUDA/GPU training Production-ready / widely deployed
NVIDIA's strongest moat
DGX/datacenter infrastructure
Production-ready Mature training platform
Omniverse/Isaac Sim Commercial/laboratory mature
Valuable for robotics development, but simulation fidelity remains task-dependent
Isaac Lab Laboratory-ready / increasingly commercial
Strong RL/policy-learning workflow
Synthetic-data pipeline
Early commercial Useful, but cannot eliminate real-world validation
Cosmos world models
Research/developer stage Strategically important, not yet a physics oracle
GR00T Lab-ready / early integration
Rapidly improving; broad production reliability still unproven
Jetson/Thor robot inference
Commercial hardware platform
Strong edge positioning
GR00T itself has evolved unusually fast: N1 in March 2025 introduced the dual-system VLM/action architecture; N1.5 improved the action model and cross-attention; N1.6 upgraded the visual/reasoning stack; GR00T 1.7 in 2026 uses a newer Cosmos-Reason-based backbone and a much larger training mixture.
NVIDIA's own synthetic-data numbers should be treated as vendor demonstrations. For N1, NVIDIA reported generating 780,000 synthetic trajectories—described as 6,500 equivalent hours—in 11 hours and reported a 40% improvement when synthetic and real data were combined in its experiment. That is evidence that the pipeline can work; it is not a universal “40% synthetic-data law.”
Intel's opportunity
Intel's 2026 robotics position is more interesting than a simple “NVIDIA competitor” framing suggests.
Existing Intel activity: Intel introduced OpenVINO Physical AI, an open-source robotics deployment framework providing a common interface across sensors, cameras, policies and robot actions, with heterogeneous execution across Intel CPU, GPU and NPU resources. The 2026 release includes camera interfaces, SO-101 and WidowX integrations, synchronous/asynchronous policy execution and OpenVINO/ ONNX backends.
Intel also positions Core Ultra Series 3 as an edge-robotics SoC combining CPU, GPU and NPU; at Computex 2026 Intel said more than 130 companies were adopting or testing Series 3 designs across edge applications. The adoption number is Intel's own claim, but the product direction is strategically relevant: an
60
61
62
integrated SoC is attractive where a robot cannot tolerate a large discrete GPU's power and thermal envelope.
At the datacenter end, Gaudi 3 remains a generative-AI training/inference accelerator with 128 GB HBM2e and dedicated tensor/matrix engines.
Intel strength / weakness / opportunity matrix
Area Position Reasoning
x86 robot/industrial host compute
Strong Huge software ecosystem and good control/AI coexistence
Integrated edge CPU+GPU+NPU
Potentially strong
Power-conscious heterogeneous execution fits many service/industrial robots; OpenVINO now targets this explicitly.
AI deployment/ compiler stack
Strong but underrecognized
OpenVINO is mature and now extends into policy runtime
Frontier robot foundation model
Weak/gap No public Intel model comparable in mindshare/ capability to Gemini Robotics, GR00T or π-family
Robotics simulation Weak/gap No Isaac/Omniverse-equivalent end-to-end ecosystem
Robotics developer mindshare
Weak versus NVIDIA
CUDA/Jetson/Isaac/GR00T form a much tighter community funnel
Robot perception sensor ownership
Weaker than historically
RealSense spun out in 2025.
Datacenter AI training
Challenger Gaudi is relevant, but software/network ecosystem competition is intense
Foundry + advanced packaging
Strategic option Robot SoCs need heterogeneous compute, chiplets, memory and I/O integration; this is an opportunity rather than a demonstrated Physical-AI franchise
Ethernet/networking Strategic option Important for training clusters and robot fleets
FPGA/real-time acceleration
Niche opportunity
Deterministic sensor/control/safety workloads can complement generative AI
Physical-AI safety runtime
High-value opening
A vendor-neutral layer for deterministic action limits, observability and fallback could differentiate OpenVINO Physical AI
My analysis: Intel is unlikely to out-NVIDIA NVIDIA by reproducing CUDA + Omniverse + Isaac + Cosmos + GR00T feature-for-feature. A more defensible strategy would be to make heterogeneous, open, low-power robot inference and safety integration exceptionally easy: x86 compatibility, CPU/GPU/NPU scheduling,
63
64
65
56
deterministic action runtime, ROS/LeRobot integration, strong ONNX/PyTorch export, common camera APIs and reference designs that run useful 0.5B–7B VLAs within robot thermal envelopes.
That becomes especially compelling if VLA inference shifts from a giant monolithic model toward a compact on-device action model plus a less frequently invoked large reasoner, which Google's separation of embodied reasoning and motor execution already suggests.
State of the art in 2026
The most important discipline in evaluating Physical AI is to ask:
Was this a research benchmark, a selected demonstration, an unedited demonstration, a customer pilot, a paid production deployment, or a broadly replicated capability?
Those categories are often conflated.
Technology-readiness assessment
Capability Approximate 2026 maturity
Why
Conventional object recognition
Widely deployed Mature CV foundation
Open-vocabulary perception
Production-ready / early widespread
Modern VLMs support broad semantic recognition/grounding; Qwen and Gemini-class systems demonstrate rich visual understanding.
Natural-language robot commands
Laboratory-ready / early commercial
Multiple VLAs support language conditioning, but execution robustness is task/environment dependent.
Tabletop manipulation
Laboratory-ready to early commercial
Strong demos and research systems; constrained picking already commercial
Mobile manipulation
Early prototype / early commercial in narrow workflows
Figure, Digit and Atlas are moving into real industrial contexts.
Warehouse manipulation
Commercial for bounded tasks
Traditional systems mature; learned systems expanding object/task variety
Industrial assembly Early commercial for selected operations
Humanoid deployments remain narrow compared with conventional automation
General household robot
Research / early prototype
Impressive home demos, insufficient reliability evidence for general deployment
66
67
68
6
Capability Approximate 2026 maturity
Why
Humanoid wholebody autonomy
Advanced prototype / early commercial
Helix 02, Atlas, Digit show major progress; broad generality remains unproven.
Long-horizon autonomous planning
Research / prototype Multi-minute demos now exist, but compounding error remains severe
Dexterous multifinger manipulation
Laboratory-ready Strong demos but contact-rich reliability remains difficult
Tool use Research / lab-ready Semantic knowledge strong; precision/contact/ recovery harder
Learning new task from a few demos
Laboratory-ready π0/OpenVLA/Octo and Gemini adaptation demonstrate the principle, not universal few-shot learning.
Cross-robot generalization
Research / lab-ready OXE and Gemini provide evidence, but morphology differences remain a major problem.
Autonomous lifelong learning
Research Safety, catastrophic forgetting and online data quality unresolved
Why long horizons are so difficult
Suppose an individual manipulation step succeeds with probability , which sounds excellent. If a task requires 50 effectively independent critical steps:
At 100 steps:
This calculation is deliberately simplistic—failures are not independent—but it illustrates the compounding reliability problem. A home robot needs success probabilities much closer to industrial-control levels, plus detection and recovery when something goes wrong.
Failure modes
Perception errors remain: occlusion, reflections, transparent objects, poor illumination and ambiguous depth.
Spatial errors matter more in robotics than VQA. “Approximately to the left” can be semantically acceptable and physically disastrous.
69
70
71
p = 0.98
P (complete) ≈ 0.98 ≈50 36.4%
0.98 ≈100 13.3%
Distribution shift occurs when furniture, camera calibration, object geometry, clothing, lighting or floor friction differs from training.
Behavior cloning suffers from covariate shift: once the robot deviates from expert states, it must act in regions where it has little demonstration data.
Action chunks can accumulate error; very short chunks increase compute and latency.
Language models can hallucinate task assumptions.
Physical understanding is incomplete: learned systems often exploit visual correlation more readily than robust mechanics.
Safety requires protection against rare catastrophic actions, not merely good average benchmark success.
Latency matters because an obsolete action based on a 300-ms-old scene may be wrong even if its underlying reasoning was correct.
VLA versus classical robotics
Classical robotics remains superior when the environment is known, precision is strict, safety certification matters and the task is repetitive. A calibrated industrial arm running a deterministic trajectory can repeat a welding motion all day with accuracy and predictable timing that current generative policies do not match.
Learned policies dominate when the hard part is variation: unfamiliar objects, deformable items, natural-language commands, complex perception, changing scene layouts or task combinatorics.
The strongest near-term systems therefore look hybrid:
There is also a genuine competing viewpoint represented by systems such as Helix 02: enough data and a sufficiently capable learned whole-body policy may absorb locomotion, manipulation and coordination into a single network. Figure's four-minute dishwasher demonstration is technically important because it tests this end-to-end hypothesis. But it remains a proprietary company demonstration, and the broader field has not yet established that end-to-end control is safer, easier to certify or more data-efficient than hierarchical alternatives.
Google's 2026 architecture provides evidence for the opposite viewpoint: Gemini Robotics-ER 2 can act as a high-level embodied “brain,” reason through multi-step tasks and hand motor execution to lower-level VLAs.
My expectation for 2026–2030 is consequently learned hierarchy rather than pure monolith: large multimodal reasoner → compact robot foundation policy → conventional safety/control substrate.
Scaling laws for robotics
VLM/VLA intelligence + geometric verification + classical control + safety supervisor
16
72
There is evidence for positive scaling, but not yet an LLM-like universal scaling law.
RT-1 demonstrated benefits from a large multi-task robot dataset.
Open X demonstrated that pooling heterogeneous robot data can improve transfer instead of inevitably causing negative interference.
Octo showed that a relatively compact model pretrained across heterogeneous episodes can become a useful initialization across new observation/action configurations.
DROID showed measurable improvements when environmental diversity was increased.
π0 and its successors pursue the hypothesis that large multimodal priors plus diverse robot experience produce increasingly general policies.
But robotics has factors absent from language scaling:
It depends on embodiment diversity, task diversity, state-space coverage, failure coverage, camera geometry, action normalization, teleoperator quality, temporal frequency and environmental variety. Ten thousand hours of one arm moving boxes in one warehouse may be less useful for generality than a much smaller but carefully diversified mixture.
The hypothesis—
“A sufficiently large and diverse robot dataset may produce a general-purpose robot foundation model”
—is plausible and increasingly supported, but not yet proven.
Evidence for it: OXE cross-robot transfer, Octo transfer, π-family scaling, GR00T's heterogeneous mixture and Gemini's multi-embodiment results.
Evidence against treating it as solved: models still need embodiment-specific adaptation, action spaces differ dramatically, dexterous contact is data hungry, long-horizon errors compound, and public benchmark success does not yet translate into arbitrary real-world task reliability.
Humanoid robotics and learning
Company 2026 public evidence of learned/foundation-model control
Assessment
Figure Helix 02 described as pixels-to-whole-body VLA; industrial BMW and home demos.
Very high learned-policy emphasis; claims remain largely proprietary
34
37
38
51
73
effective data = raw hours alone
74
43
Company 2026 public evidence of learned/foundation-model control
Assessment
Boston Dynamics
RL partnership with RAI; Atlas uses Jetson Thor/ Isaac; 2026 Gemini Robotics partnership.
Hybrid: exceptionally strong classical/control heritage plus aggressive learning adoption
Agility Robotics Digit whole-body-control foundation model, NVIDIA simulation stack, paid Toyota agreement.
Hybrid learned whole-body + industrial deployment
Apptronik Apollo 2 Robot Park fleet collecting real-world data with Google DeepMind partnership.
Foundation-model/data strategy increasingly central
Mentee/ Mobileye
Humanoid architecture centered on mentoring, few-shot learning and simulation-first training.
AI-first / Physical-AI emphasis
Unitree
H2 supports high-capacity onboard embodied models and substantial compute; public product material emphasizes motion algorithms and model support.
Strong learning platform; exact general-policy architecture less public
Tesla Optimus
Public presentations indicate large-scale learning/ data ambitions, but detailed current VLA architecture is not sufficiently publicly specified to make a rigorous RT-2/GR00T-style comparison
Do not over-infer
1X / Sanctuary / Fourier / UBTECH
All are relevant general-purpose/humanoid players, but public technical disclosure varies considerably
Company-by-company verification required before claiming a specific VLA architecture
A useful warning: the phrase “AI-powered humanoid” does not imply “end-to-end VLA.” A humanoid can combine RL locomotion, hand-designed state machines, learned perception, classical whole-body control and an LLM planner. Public evidence should be used before assigning an architecture.
Israel's Physical AI ecosystem
Israel's position is best described as strong upstream intelligence and autonomy, weaker downstream robot-scale manufacturing and robot-data accumulation.
The country already possesses several components required for a Physical-AI cluster: advanced computer vision and ML research, world-class autonomous-vehicle expertise, defense autonomy, semiconductor design, strong startup formation, and substantial multinational R&D. What it lacks relative to the United States and China is a large domestic base of robot manufacturers deploying tens of thousands of generalpurpose manipulators/humanoids that automatically create a gigantic data flywheel.
75
76
77
78
79
Academic map — representative, not exhaustive
Researcher / group Institution Public focus Connection to VLM/VLA/ Physical AI
Vadim Indelman — Autonomous Navigation and Perception Lab
Technion
Probabilistic perception, SLAM, planning under uncertainty, single/multi-robot autonomy; current 2026 work includes continuous MDP/POMDP planning.
Directly relevant to embodied autonomy, especially state estimation/ planning rather than VLA pretraining
Kiril Solovey — Multi-Robot Systems Lab
Technion ECE
Planning, control, coordination, autonomous driving/racing, multi-robot systems.
Strong classical/planning complement to foundation agents
Control, Robotics & Machine Learning researchers
Technion Robot learning, reinforcement learning, vision and control.
Direct robot-learning relevance
Ayellet Tal / Lihi Zelnik-Manor and computer-graphics/vision groups
Technion 2D/3D vision, recognition, learning and geometric understanding.
Upstream VLM/spatial-perception relevance
Elon Rimon — Robot Navigation Lab
Technion Robot geometry, navigation and legged systems.
Classical physical reasoning/ control complement
Lior Wolf — Deep Learning Lab
Tel Aviv University
Vision, video, languagerelated deep learning, RL/ robotics; also CEO of Mentee Robotics.
One of Israel's clearest academia→Physical-AIbridges
Amir Globerson — AI Lab
Tel Aviv University
Machine learning, complex scenes, vision-language and reasoning.
Direct VLM/multimodal relevance
Goren Gordon — Robotic Augmentation Lab
Tel Aviv University
Physical agents and human-robot interaction.
Embodied interaction / robotics
Vision and motorcontrol researchers including Shimon Ullman, Ronen Basri, Michal Irani, Tamar Flash and associated groups
Weizmann Institute
Vision, scene analysis, motor control and model-based AI are represented in Weizmann's AI/robotics research portfolio.
Strong underlying vision/ representation/control base
80
81
82
83
84
85
86
87
88
Researcher / group Institution Public focus Connection to VLM/VLA/ Physical AI
Chaim Baskin — INSIGHT Lab
Ben-Gurion University
Efficient deep learning and multimodal/vision methods.
Edge/model-efficiency relevance
ABC Robotics initiative Ben-Gurion University
Agricultural, biological and cognitive robotics.
Embodied robotics testbeds/ applications
Gal Kaminka and Robotics Consortium
Bar-Ilan University
Multi-robot systems, autonomous agents, RoboCup and cooperation; consortium includes multiple robotics researchers.
Multi-agent embodied autonomy
Computer Vision & Deep Learning community
University of Haifa
Foundation/multimodal learning, computer vision; parallel work on human behavior/motion.
More upstream multimodal/ vision than VLA
HCI/HRI researchers Reichman University
Human-computer and human-robot interaction, applied AI.
Adjacent embodiedinteraction layer
The pattern matters: Israel has many of the individual scientific competencies, but far fewer labs than Stanford/Berkeley/CMU/DeepMind-style ecosystems publicly training million-trajectory general robot policies. That makes coordination and shared infrastructure especially important.
Israeli startups and scale-ups
Company Founding / people
Technology Funding / status Technical significance
Mentee Robotics
Founded 2022; associated with Amnon Shashua, Lior Wolf and Shai Shalev-Shwartz
Humanoid robot, human-to-robot mentoring, fewshot learning, simulation-first training
Mobileye announced acquisition in Jan. 2026; total consideration approximately $900M; Mentee operates as an independent Mobileye unit.
Israel's strongest current generalpurpose humanoid/ Physical-AI asset
89
90
91
92
93
94
Company Founding / people
Technology Funding / status Technical significance
Enigma
Reported founded 2025 by Jonathan Jacobi and Gal Niv
Robot foundation models, robotagnostic abstraction layer and interaction interface
Reported $71M seed in Jul. 2026 led by Index Ventures/ Ribbit with additional investors; official site confirms robot-agnostic foundation-model focus.
Potentially the closest new Israeli startup to a pure robot-foundation-model company
Pickommerce AI Robotics
IIA lists Kfir Nissim, CEO/ cofounder, and Amir Shapiro, CTO/ cofounder; established 2021 in IIA records
AI/computer-vision-driven autonomous pick-and-pack/ palletizing
Israel Innovation Authority-backed; company reports deployment at Teva logistics
Practical Physical AI in unstructured logistics rather than a general VLA.
Gangelia / StochastiX
Tel Aviv
Simulation/ domain randomization/ fault injection and VLA/VLM task adaptation for humanoids
Early-stage; public financial data not established here
Interesting infrastructure layer rather than robot manufacturer; current performance statements are self-reported.
Robotican Israeli robotics company
Autonomous ground/air systems, simulation/digital-twin tools
Operating company
Adjacent autonomy/ simulation capability rather than general VLA
Skyline Robotics
Tel Aviv-origin industrial robotics company
Autonomous highrise windowcleaning robotics
Commercial industrial automation
Example of task-focused Physical AI, not robot foundation models
A useful distinction is foundation-model startup versus AI-enabled robotics startup. Enigma and Mentee belong near the former category. Pickommerce and Skyline solve bounded industrial problems with sophisticated perception/action technology. Both are economically important, but only the first category is trying to create a reusable “intelligence layer” across many robot tasks.
95
96
97
Mobileye's Mentee acquisition is strategically significant beyond the transaction. Mobileye explicitly framed the deal as combining its automotive autonomy stack—perception, context/intent reasoning and safety— with humanoid Physical AI. Its public plan called for initial on-site Mentee proof-of-concept deployments in 2026 and targeted series production/commercialization in 2028; those dates are forward-looking company targets, not guaranteed milestones.
Multinational activity in Israel
It is important not to overstate what can be verified publicly. NVIDIA, Intel, Google, Microsoft, Amazon, Meta, Apple and Qualcomm all have substantial global technologies relevant to multimodal AI or computing, but the existence of an Israeli R&D center does not by itself establish that Gemini Robotics, GR00T, Claude-like multimodal research, or another VLA is being developed in Israel.
The clearest directly documented Israel-specific connection in the sources reviewed is Mobileye/Mentee. Intel has a globally relevant Physical-AI runtime via OpenVINO, but the public material reviewed does not establish that this work is led by Intel Israel. Likewise, NVIDIA's Israeli engineering footprint should not be conflated with evidence that GR00T or Cosmos core model development is Israel-based.
That distinction is important for ecosystem mapping: otherwise almost every multinational AI project could incorrectly be counted as Israeli activity.
Government and national infrastructure
Israel's most important public AI-infrastructure step in 2026 is the national-supercomputer access program. The Israel Innovation Authority says the Nebius-operated infrastructure is live and will allocate resources totaling 1,000 NVIDIA B200 accelerators over coming years, with 70% allocated to industry and 30% to academia at discounted rates.
That is highly relevant to VLM/VLA research because training and fine-tuning foundation models is precisely the type of workload for which individual academic labs and early-stage startups otherwise struggle to acquire sufficient accelerator capacity.
The Ministry of Defense has also established a formal AI and Autonomy organization within DDR&D. By late 2025 its leadership publicly described deployment of tens of thousands of autonomous systems across domains and collaboration among the IDF, startups, defense companies and research institutions. The operational claims concern defense systems broadly—not necessarily VLAs—but they demonstrate a large domestic demand side for embodied autonomy.
Israel's Innovation Authority also operates infrastructure/laboratory programs and vouchers designed to give companies access to advanced R&D facilities and compute.
Is there an explicit Israeli Physical-AI strategy?
Finding: I did not find a publicly named national civilian “Physical AI” strategy equivalent to a dedicated robot-foundation-model or humanoid initiative in the official material reviewed.
98
99
100
101
Inference: Israel already has much of the substrate from which such a strategy could be assembled:
\text{National AI compute} + \text{AI/autonomy defense R&D} + \text{academic robot learning} + \text{Mobileye/Mentee} + \text{industrial robotics startups} + \text{semiconductor expertise}
The missing pieces are a coordinated national robot-data program, standardized cross-university hardware platforms, large-scale civilian robot testbeds and stronger domestic robot manufacturing.
Israel versus the world
Dimension US China Europe Japan South Korea Israel
Frontier multimodal foundation models
Very strong
Very strong Moderate/ strong pockets
Moderate Moderate
Strong research, limited frontiermodel scale
General robot foundation models
Global leader
Rapidly expanding
Strong research pockets
Strong robotics, less visible foundationmodel leadership
Growing Emerging
Humanoid manufacturers
Very strong
Extremely dense / cost competitive
Emerging Established robotics heritage
Strong industrial base
Weak manufacturing base
Computer vision
Very strong
Very strong Very strong research
Strong Strong Global-strength niche
Autonomous vehicles
Strong Strong Strong Strong Strong
Exceptional relative strength via Mobileye ecosystem
Semiconductors Very strong
Growing Specialized Strong Very strong memory/ manufacturing
Strong design/ R&D, limited domestic fullstack scale
Defense/ autonomous systems
Very strong
Very strong Strong More limited
Strong Major relative strength
Dimension US China Europe Japan South Korea Israel
Access to large robot datasets
Strong Potentially very strong
Moderate
Strong industrial data but often closed
Growing Weakness
Startup density Very strong
Strong Fragmented Moderate Growing Very strong per capita
This is an analytical comparison rather than a quantitative ranking. Israel's highest-probability route to global leadership is not likely to be competing with China on low-cost humanoid manufacturing volume. More credible opportunities are robot intelligence, embodied perception, safety, autonomy, edge silicon/software, industrial manipulation and defense-to-civilian technology transfer.
A particularly attractive Israeli niche would be small, efficient, safety-aware robot foundation models deployable at the edge. That combines the country's existing strengths in CV, autonomous vehicles, edge computing and semiconductors while avoiding a direct race to own the largest humanoid factory.
Reading, tools, curriculum, and the end-to-end example
The following reading list is deliberately biased toward papers that establish a concept or changed how robot systems are built. ★ marks the approximately ten papers I would read first.
Essential papers and technical reports
Area Paper Year Main contribution / why it matters
Vision-language ★ Learning Transferable Visual Models From Natural Language Supervision — CLIP
2021 Established large-scale contrastive image-language pretraining and zero-shot visual transfer.
Vision-language ★ Flamingo: a Visual Language Model for Few-Shot Learning
2022 Interleaved text/image/video fewshot VLM with frozen backbones and cross-modal fusion.
Vision-language BLIP: Bootstrapping Language-Image Pre-training
2022 Unified vision-language understanding/generation and data bootstrapping.
Vision-language ★ BLIP-2 2023 Q-Former efficiently connects frozen vision and language models.
Vision-language PaLI: A Jointly-Scaled Multilingual Language-Image Model
2022 Large multitask multilingual encoder-decoder VLM.
21
22
23
25
Area Paper Year Main contribution / why it matters
Vision-language ★ Visual Instruction Tuning — LLaVA
2023 Demonstrated simple, scalable visual instruction tuning; foundational to open MLLMs.
Vision-language Qwen-VL 2023
Strong open multimodal model lineage with grounding; later Qwen2.5-VL made this highly practical.
Embodied FM ★ PaLM-E: An Embodied Multimodal Language Model
2023 Major bridge between language models, visual input and embodied tasks.
Generalist agent
A Generalist Agent — Gato 2022 Demonstrated one Transformer across text, images, games and robot control.
Embodied planning
★ Do As I Can, Not As I Say — SayCan
2022 Combines language-model planning with learned robotic affordance/ value functions.
Robot policy ★ RT-1: Robotics Transformer for Real-World Control at Scale
2022 Large multi-task Transformer robot policy and action tokenization.
Imitation
★ Learning Fine-Grained Bimanual Manipulation with Low-Cost Hardware — ACT/ ALOHA
2023 Action chunking and inexpensive bimanual data collection.
Diffusion policy ★ Diffusion Policy 2023 Made diffusion over action trajectories a major robot-policy paradigm.
Generalist robotics
RoboCat 2023 Multi-task/multi-embodiment self-improving robotic agent.
VLA ★ RT-2: Vision-Language-Action Models Transfer Web Knowledge to Robotic Control
2023 Formalized web-pretrained VLM → direct action-token VLA.
Generalist robot data
★ Open X-Embodiment 2023/24 Cross-institution, cross-embodiment dataset and RT-X evidence.
Generalist policy
Octo: An Open-Source Generalist Robot Policy
2024 Compact transformer-diffusion foundation policy pretrained on OXE.
Open VLA ★ OpenVLA 2024 Made a large VLA practically open and fine-tunable.
26
34
35
102
36
12
37
38
32
Area Paper Year Main contribution / why it matters
Robot data DROID: A Large-Scale In-the-Wild Robot Manipulation Dataset
2024 Standardized multi-institution realworld robot data collection.
Robot data BridgeData V2 2023 Large, diverse WidowX manipulation data; important ingredient in open VLA work.
Robot data RoboNet 2019 Early large multi-robot video/action dataset demonstrating cross-robot policy-data pooling.
Human video Ego4D 2021 Large egocentric human-video resource relevant to human-to-robot transfer.
Robot prompting
VIMA 2022 Multimodal prompts for general robot manipulation.
Embodied reasoning
VoxPoser 2023 Uses language models and 3D value maps to compose robot actions.
VLA π0: A Vision-Language-Action Flow Model for General Robot Control
2024 Combines VLM semantic knowledge with flow-matching high-frequency continuous action generation.
Efficient VLA SmolVLA 2025 Compact 450M open VLA oriented toward commodity hardware.
Open robotics tooling
LeRobot: An Open-Source Library for End-to-End Robot Learning
2026 Standardized open-source data/ policy/hardware stack.
World model ★ World Models 2018 Canonical modern formulation of learning a compact predictive model for control.
World model Learning Latent Dynamics for Planning from Pixels — PlaNet
2018 Planning in a learned latent dynamics space.
World model Dream to Control — Dreamer 2019 Learned latent imagination for policy improvement.
World model Mastering Diverse Domains through World Models — DreamerV3
2023 Demonstrates unusually general model-based RL recipe.
Generative world model
★ Genie: Generative Interactive Environments
2024 Learns action-controllable worlds and latent actions from unlabeled video.
51
103
40
57
54
Area Paper Year Main contribution / why it matters
Video-to-control Dreamitate 2024 Uses video generation as an intermediate representation for robot control.
Embodied simulation
Habitat 2019 Standard embodied-navigation simulation platform.
Simulation Isaac Gym 2021 GPU-parallel physics/RL training; precursor to modern Isaac learning workflows.
Manipulation benchmark
ManiSkill 2021 Large manipulation simulation/ benchmark ecosystem.
NVIDIA robot FM
GR00T N1 technical project 2025 Open humanoid foundation-policy architecture combining reasoning and continuous action.
NVIDIA world model
DreamGen 2025 Explores world-model-generated trajectories for robot-policy training.
Physical Intelligence
π0.7 and current research 2026 Current PI direction: steerability, memory, RL and broad generalization.
Google robot FM
Gemini Robotics family 2025–
26
Current embodied-reasoning/VLA architecture and whole-body direction.
General physical reasoning
Genie / generative visual foundations lineage
2024– 26
Important bridge from video generative models to interactive world modeling.
The ten I would prioritize are: CLIP, Flamingo, BLIP-2, LLaVA, PaLM-E, SayCan, RT-1, ACT, Diffusion Policy, RT-2, Open X-Embodiment, π0 and Genie. That is slightly more than ten because the sequence is more educational than an arbitrary cutoff.
Open-source ecosystem
Tool Best use Beginner suitability
LeRobot End-to-end data collection, policy training, VLAs, hardware interfaces
Best overall starting point; current repo integrates an unusually broad model set.
OpenVLA Understanding a classic large autoregressive VLA
Intermediate; excellent codebase for VLA internals.
104
105
106
44
107
108
57
32
Tool Best use Beginner suitability
openpi π0/π0-FAST/π0.5-family experiments
Advanced; ideal after ACT/OpenVLA basics.
ACT/ALOHA Action chunking, low-cost bimanual imitation
Excellent robot-learning introduction.
Diffusion Policy Continuous generative robot policies
Intermediate
SmolVLA Small VLA on consumer hardware Excellent bridge into foundation policies.
MuJoCo Accurate fast rigid-body simulation Essential
Isaac Sim / Isaac Lab
GPU simulation, synthetic data, RL, NVIDIA stack
Intermediate/advanced
ManiSkill Manipulation benchmarks/ simulation
Intermediate
Habitat Embodied navigation Intermediate
ROS 2 Robot middleware, messaging, drivers
Essential practical robotics
MoveIt Motion planning/manipulation Essential for hybrid VLM + classical stack
DROID Real robot dataset, hardware and data standardization
Advanced but exceptionally educational.
Open X-Embodiment
Cross-robot dataset pretraining Research-scale
OpenVINO Physical AI
Deploying learned policies/VLAs on Intel heterogeneous hardware
Particularly relevant for edge/ semiconductor work.
Progressive self-study curriculum
Phase Concepts Core reading/ software
Practical exercise Hardware Complexity
Understand multimodal models
CLIP, ViT, projector, cross-attention, grounding
CLIP, Flamingo, BLIP-2, LLaVA
Embed images/ text; inspect similarity and grounding
Any GPU/ cloud
Low– medium
58
35
40
51
109
Phase Concepts Core reading/ software
Practical exercise Hardware Complexity
Run a VLM locally
Quantization, image tokens, latency, OCR
Qwen2.5-VL / LLaVA
Webcam scene Q&A; return structured boxes/ points
8–16GB+ GPU useful
Medium
Understand robot control
Frames, SE(3), FK/ IK, Jacobians, trajectory control, PID, state estimation
Modern Robotics, ROS 2, MoveIt
Simulate a 6/7-DOF arm; Cartesian pick/ place
Laptop Medium
Build VLM + robot integration
Calibration, depth, grounding→pose conversion
ROS 2 + MoveIt + VLM
“Pick the red object” with a small arm
Camera + arm
Medium– high
Train imitation policy
BC, covariate shift, action chunks
ACT or Diffusion Policy, LeRobot
Record 50–200 demos and train a pick/place task
Low-cost arm + 12GB+ GPU
High
Run/fine-tune a VLA
VLM backbone, action head, normalization, LoRA
SmolVLA → OpenVLA → openpi
Fine-tune on own demonstrations
24GB-class GPU ideal; larger for full FT
High
Explore foundation/ world models
Cross-embodiment data, latent dynamics, human video, synthetic data, RL
OXE, π-family, GR00T, Genie, Cosmos
Compare direct VLA versus model-predictive/ world-model planning in sim
Lab/ datacenter depending model
Very high
For courses, an experienced engineer benefits more from Modern Robotics for kinematics/control plus the hands-on LeRobot robot-learning tutorial than from beginning with another generic deep-learning course. LeRobot's current project explicitly includes a free robot-learning tutorial and an end-to-end browser-based LeLab workflow.
End-to-end example: “Pick up the red screwdriver and place it next to the blue box.”
Assume a mobile or fixed arm with an RGB/depth camera.
Camera sensor. The RGB sensor creates an image; depth/stereo may produce range. Calibration maps image pixels into the robot/base coordinate frame.
Vision encoder. A ViT divides the image into patches. Attention produces feature vectors containing information about objects, texture, text, geometry and context.
110
Multimodal fusion. The instruction
“Pick up the red screwdriver and place it next to the blue box.”
is tokenized. Vision representations and language tokens are brought together through a projector, crossattention or unified multimodal Transformer.
Semantic grounding. The model must bind three things:
A VLM may return points/boxes for both objects. A VLA may never explicitly expose these labels; the grounding can remain latent.
Spatial reasoning. The system determines which visible elongated red object is the screwdriver, which object is the blue box, the candidate grasp region and a free placement region adjacent to the box.
Task planning. A high-level model might produce latent or explicit subgoals:
Gemini Robotics-ER-style architectures make this high-level reasoning/planning stage relatively explicit.
World-model role. An explicit world model could simulate or score alternatives: “Will grasping the screwdriver at its tip make it rotate?” “Will this trajectory hit the mug?” “Is there enough free table area next to the blue box?” In many current systems, this prediction is instead implicit in the policy.
Action generation. Suppose the VLA uses end-effector chunks:
It may predict 10–50 such vectors at once. ACT would produce a trajectory chunk directly; a diffusion policy would denoise a candidate sequence; π0-style flow matching generates continuous actions; RT-2/OpenVLA-style systems can decode action tokens.
Traditional motion/control layer. If the VLA outputs Cartesian pose targets, a classical IK solver maps them to joint targets. A motion planner can reject collisions. A safety layer clamps velocity and workspace. The low-level controller tracks joint targets at hundreds or thousands of hertz.
Motor movement. Servo amplifiers produce motor currents/torques. Encoders update joint position. None of this requires a giant Transformer.
New observation. After perhaps 100–300 ms the VLA receives a fresh image and proprioception:
source object = red screwdriver
target reference = blue box
relation = next to
approach → grasp handle → lift → move clear of obstacles → place near box → release
72
a =t [Δx, Δy, Δz, Δr , Δr , Δr , g]x y z
111
screwdriver moved only half as far as expected; gripper may have slipped.
It produces a corrected chunk.
Closed-loop completion. The cycle repeats until the screwdriver is next to the blue box. A success model or VLM may inspect the final scene.
So the full path is:
That diagram is probably the most durable way to understand Physical AI: the VLM gives semantic grounding, the VLA gives embodied policy, the world model provides predictive imagination, and classical robotics provides deterministic physical execution and safety where it remains superior.
The major open research questions for 2026–2030
The first is data economics. The industry must discover whether real robot demonstrations, simulation, human video and self-generated experience can be mixed cheaply enough to support billion-scale useful trajectories.
The second is embodiment transfer. An action for a Franka arm is not an action for a humanoid hand. General robot models need representations above raw joint coordinates while retaining precision below them. Open X is encouraging evidence, but not a final solution.
The third is long-horizon reliability. Memory, hierarchical planning and learned recovery are likely at least as important as another 2× increase in model parameters. Physical Intelligence's 2026 research direction explicitly includes longer- and shorter-term embodied memory, illustrating how central this has become.
The fourth is physics. Models need not reconstruct symbolic Newtonian mechanics internally, but they must predict contact, support, friction, deformables, liquids and object permanence accurately enough for action.
The fifth is safety architecture. A generative policy should probably not be allowed to command arbitrary torque without independent constraints. Research is needed on action shields, runtime verification, uncertainty, fallback controllers and safe online learning.
The sixth is continual learning. A useful home/factory robot should become better after deployment without catastrophically forgetting prior skills or turning a rare bad interaction into a bad policy.
Camera + depth
Vision encoder
Language instruction
Multimodal fusion Grounding + spatial reasoning
Proprioception
VLA policyTask plan / subgoal
Optional world model predict consequences
Action chunk Safety / collision / IK as needed Real-time controller Motors / gripper
Physical world
37
44
The seventh is evaluation. Robotics lacks the convenience of a text benchmark containing millions of instantly reproducible queries. Real-world evaluations are expensive and often proprietary, allowing impressive selected demonstrations to outrun rigorous statistics.
The eighth is world-model utility. It is not yet clear how much explicit video/latent prediction will improve real robot decisions relative to simply making the policy larger and better trained.
The ninth is systems architecture: whether large intelligence lives in one model or is divided into high-level multimodal reasoning, compact motor policies, world models and deterministic safety systems.
The tenth is inference efficiency. General robots cannot assume a kilowatt-class GPU cluster. Progress in quantization, compact VLMs, action chunking, asynchronous inference, memory hierarchy and heterogeneous NPUs will determine whether Physical AI becomes economically deployable.
No paywalled source was necessary for the core conclusions in this report; where journal work was relevant, open preprints or project pages were available.
What I Should Remember
Core insight Durable mental model
A VLM sees and talks; a VLA sees, understands an instruction, and acts.
VLA = multimodal semantics crossing into the control loop.
A VLA is a policy, not a training algorithm.
It can be trained by behavior cloning, RL, offline RL or combinations.
Diffusion/flow/autoregressive tokens are alternative ways to represent the action distribution.
Do not confuse model modality with action decoder.
Robot state matters as much as camera input.
Proprioception tells the policy where its body actually is.
Action abstraction determines compute and transferability.
Joint torque is precise but morphology-specific; Cartesian deltas transfer more easily.
Action chunking is one of the field's most important practical ideas.
Predict several future controls per expensive neural inference.
Closed-loop control is nonnegotiable for robust physical behavior.
Observe → act → observe again.
Foundation models do not make classical robotics obsolete.
Safety, servo control, geometry and state estimation remain valuable.
Core insight Durable mental model
The likely winning stack is hierarchical.
Multimodal reasoner → VLA → deterministic safety/control.
Robot data is the scarce resource. Internet data can be copied; physical trajectories must be generated.
Cross-robot data pooling works enough to matter.
Open X, Octo and successors show positive transfer, though universal embodiment transfer remains unsolved.
Human video is potentially a gigantic prior, not a full substitute for robot experience.
It provides semantic/action structure without exact robot motor labels.
Simulation is becoming part of the data factory.
Real data calibrates simulation; simulation creates coverage; real robots validate.
A world model predicts consequences; a policy chooses actions.
“What happens if I push?” versus “Push now.”
Generative video models are proto-world-model technology, not trustworthy physics engines yet.
Visual plausibility is not physical accuracy.
2026 VLAs are impressive but not generally reliable robots.
Selected multi-minute demos and narrow commercial deployments coexist with major robustness gaps.
Google, NVIDIA, Physical Intelligence and Figure represent four different strategies.
Google: multimodal intelligence; NVIDIA: full platform; PI: general robot models; Figure: vertically integrated humanoid + data fleet.
For semiconductors, inference and training are different markets.
HBM-scale datacenter compute trains; power-efficient heterogeneous SoCs operate robots.
Intel's best Physical-AI opening is not merely another huge accelerator.
Open, heterogeneous, power-efficient robot inference plus deterministic safety integration is a credible differentiator. OpenVINO Physical AI is a concrete first step.
Israel has strong ingredients but a robot-data/manufacturing gap.
Its advantage is intelligence, vision, autonomy, semiconductors and startups—not yet fleet scale. Mobileye/Mentee, Enigma, national AI compute and defense autonomy could form the nucleus of a larger Physical-AI cluster.
https://openai.com/index/clip/ https://openai.com/index/clip/
https://robotics-transformer1.github.io/ https://robotics-transformer1.github.io/
68
112
113
109
114
1 11 21
2 18 31 34
https://www.figure.ai/news/helix-02 https://www.figure.ai/news/helix-02
https://robotic-transformer-x.github.io/ https://robotic-transformer-x.github.io/
https://deepmind.google/blog/gemini-robotics-2-brings-whole-body-intelligence-to-robots/ https://deepmind.google/blog/gemini-robotics-2-brings-whole-body-intelligence-to-robots/
https://www.figure.ai/news/f-03-at-bmw https://www.figure.ai/news/f-03-at-bmw
https://ir.mobileye.com/news-releases/news-release-details/mobileye-acquire-mentee-robotics-accelerate-physical-ai https://ir.mobileye.com/news-releases/news-release-details/mobileye-acquire-mentee-robotics-accelerate-physical-ai
https://innovationisrael.org.il/en/press_release/supercomputer-access-2026/ https://innovationisrael.org.il/en/press_release/supercomputer-access-2026/
https://deepmind.google/blog/rt-2-new-model-translates-vision-and-language-into-action/ https://deepmind.google/blog/rt-2-new-model-translates-vision-and-language-into-action/
https://deepmind.google/models/model-cards/gemini-3-5-flash/ https://deepmind.google/models/model-cards/gemini-3-5-flash/
https://nvidianews.nvidia.com/news/nvidia-launches-cosmos-3-the-open-frontier-foundation-model-for-physical-ai https://nvidianews.nvidia.com/news/nvidia-launches-cosmos-3-the-open-frontier-foundation-model-for-physical-ai
https://www.physicalintelligence.company/blog/pi0? _bhlid=2acac8eec5eaf23f1241b374e9282a2975896b92 https://www.physicalintelligence.company/blog/pi0?_bhlid=2acac8eec5eaf23f1241b374e9282a2975896b92
https://deepmind.google/blog/gemini-robotics-er-1-6/ https://deepmind.google/blog/gemini-robotics-er-1-6/
https://www.pi.website/ https://www.pi.website/
https://deepmind.google/models/model-cards/gemini-3-5-flash-lite/ https://deepmind.google/models/model-cards/gemini-3-5-flash-lite/
https://deepmind.google/blog/tackling-multiple-tasks-with-a-single-visual-language-model/ https://deepmind.google/blog/tackling-multiple-tasks-with-a-single-visual-language-model/
https://arxiv.org/abs/2301.12597 https://arxiv.org/abs/2301.12597
https://llava-vl.github.io/blog/2024-01-30-llava-next/ https://llava-vl.github.io/blog/2024-01-30-llava-next/
https://research.google/blog/google-research-2022-beyond-language-vision-and-generative-models/ https://research.google/blog/google-research-2022-beyond-language-vision-and-generative-models/
https://qwenlm.github.io/blog/qwen2.5-vl/ https://qwenlm.github.io/blog/qwen2.5-vl/
3 16 43 69 112
4 10 37 68 71 74
5 7 46 107 113
6
8 78 94 98 114
9 99
12
13
14 55
15 39 73 103
17
19 44
20
22
23
24 48
25
26 67
https://deepmind.google/models/model-cards/gemini-3-1-flash-lite/ https://deepmind.google/models/model-cards/gemini-3-1-flash-lite/
https://deepmind.google/models/gemini/ https://deepmind.google/models/gemini/
https://openai.com/index/introducing-gpt-5-4-mini-and-nano/ https://openai.com/index/introducing-gpt-5-4-mini-and-nano/
https://developer.nvidia.com/blog/scale-synthetic-data-and-physical-ai-reasoning-with-nvidia-cosmos-world-foundation-models/ https://developer.nvidia.com/blog/scale-synthetic-data-and-physical-ai-reasoning-with-nvidia-cosmos-world-foundation-models/
https://github.com/openvla/openvla https://github.com/openvla/openvla
https://journals.sagepub.com/doi/10.1177/02783649241273668 https://journals.sagepub.com/doi/10.1177/02783649241273668
https://tonyzhaozh.github.io/aloha/ https://tonyzhaozh.github.io/aloha/
https://deepmind.google/research/publications/35829/ https://deepmind.google/research/publications/35829/
https://octo-models.github.io/ https://octo-models.github.io/
https://huggingface.co/blog/smolvla https://huggingface.co/blog/smolvla
https://nvidianews.nvidia.com/news/nvidia-isaac-gr00t-n1-open-humanoid-robot-foundation-model-simulation-frameworks https://nvidianews.nvidia.com/news/nvidia-isaac-gr00t-n1-open-humanoid-robot-foundation-model-simulation-frameworks
https://deepmind.google/en/models/gemini-robotics/gemini-robotics/ https://deepmind.google/en/models/gemini-robotics/gemini-robotics/
https://developer.nvidia.com/blog/develop-humanoid-robot-policies-end-to-end-with-nvidia-isaac-gr00t/ https://developer.nvidia.com/blog/develop-humanoid-robot-policies-end-to-end-with-nvidia-isaac-gr00t/
https://droid-dataset.github.io/ https://droid-dataset.github.io/
https://llava-vl.github.io/blog/2024-06-16-llava-next-interleave/ https://llava-vl.github.io/blog/2024-06-16-llava-next-interleave/
https://www.figure.ai/news/ramping-figure-03-production https://www.figure.ai/news/ramping-figure-03-production
https://www.figure.ai/news/project-go-big https://www.figure.ai/news/project-go-big
https://deepmind.google/research/publications/60474/ https://deepmind.google/research/publications/60474/
27
28
29
30
32
33
35 111
36
38 70
40
41 60 61
42
45
47 50 51
49
52
53
54 108
https://www.realsenseai.com/news-insights/news/realsense-completes-spin-out-from-intel-raises-50-million-to-accelerate-ai-powered-vision-for-robotics-and-biometrics/ https://www.realsenseai.com/news-insights/news/realsense-completes-spin-out-from-intel-raises-50-million-to-accelerate-ai-powered-vision-for-robotics-and-biometrics/
https://github.com/huggingface/lerobot https://github.com/huggingface/lerobot
https://github.com/Physical-Intelligence/openpi https://github.com/Physical-Intelligence/openpi
https://developer.nvidia.com/blog/advanced-sensor-physics-customization-and-model-benchmarking-coming-to-nvidia-isaac-sim-and-nvidia-isaac-lab/ https://developer.nvidia.com/blog/advanced-sensor-physics-customization-and-model-benchmarking-coming-to-nvidia-isaac-sim-and-nvidia-isaac-lab/
https://www.intel.com/content/www/us/en/developer/tools/openvino-toolkit/physical-ai.html https://www.intel.com/content/www/us/en/developer/tools/openvino-toolkit/physical-ai.html
https://newsroom.intel.com/artificial-intelligence/intel-core-ultra-series-3-for-edge-ai-robotics https://newsroom.intel.com/artificial-intelligence/intel-core-ultra-series-3-for-edge-ai-robotics
https://newsroom.intel.com/press-kit/enterprise-ai-products https://newsroom.intel.com/press-kit/enterprise-ai-products
https://deepmind.google/models/gemini-robotics/embodied-reasoning/ https://deepmind.google/models/gemini-robotics/embodied-reasoning/
https://bostondynamics.com/news/boston-dynamics-and-the-robotics-ai-institute-partner/ https://bostondynamics.com/news/boston-dynamics-and-the-robotics-ai-institute-partner/
https://www.agilityrobotics.com/content/training-a-whole-body-control-foundation-model https://www.agilityrobotics.com/content/training-a-whole-body-control-foundation-model
https://apptronik.com/news-collection/welcome-to-robot-park-where-apptroniks-apollo-goes-to-work https://apptronik.com/news-collection/welcome-to-robot-park-where-apptroniks-apollo-goes-to-work
https://www.unitree.com/mobile/H2/ https://www.unitree.com/mobile/H2/
https://anpl-technion.github.io/ https://anpl-technion.github.io/
https://mrstechnion.github.io/ https://mrstechnion.github.io/
https://crml.eelabs.technion.ac.il/ https://crml.eelabs.technion.ac.il/
https://cgm.technion.ac.il/about-us/ https://cgm.technion.ac.il/about-us/
https://meeng.technion.ac.il/labs/robot-navigation-laboratory-2/ https://meeng.technion.ac.il/labs/robot-navigation-laboratory-2/
https://en-exact-sciences.tau.ac.il/computer/research_laboratories_Deep_Learning https://en-exact-sciences.tau.ac.il/computer/research_laboratories_Deep_Learning
56
57 110
58
59
62 65 109
63
64
66 72
75
76
77
79
80
81
82
83
84
85
https://tilabs.tau.ac.il//AI_Lab https://tilabs.tau.ac.il//AI_Lab
https://tilabs.tau.ac.il/Robotic_Augmentation https://tilabs.tau.ac.il/Robotic_Augmentation
https://www.weizmann.ac.il/math/research/artificial-intelligence-ai https://www.weizmann.ac.il/math/research/artificial-intelligence-ai
https://insight.bgu.ac.il/ https://insight.bgu.ac.il/
https://www.bgu.ac.il/en/u/research-centers/robotics/abc-vision/ https://www.bgu.ac.il/en/u/research-centers/robotics/abc-vision/
https://cs.biu.ac.il/he/robotics https://cs.biu.ac.il/he/robotics
https://cvdl-club.cis.haifa.ac.il/ https://cvdl-club.cis.haifa.ac.il/
https://lps.runi.ac.il/ma-hci/ https://lps.runi.ac.il/ma-hci/
https://seedtable.com/companies/enigma-robotics/funding-rounds/seed-2026-07 https://seedtable.com/companies/enigma-robotics/funding-rounds/seed-2026-07
https://innovationisrael.org.il/en/importWinner/516521200 https://innovationisrael.org.il/en/importWinner/516521200
https://gangelia.io/ https://gangelia.io/
https://www.mod.gov.il/en/press-releases/press-room/this-is-the-first-robotics-war-tens-of-thousands-of-autonomous-systems-deployed-across-all-domains https://www.mod.gov.il/en/press-releases/press-room/this-is-the-first-robotics-war-tens-of-thousands-of-autonomous-systems-deployed-across-all-domains
https://innovationisrael.org.il/en/calls_for_proposal/usage-vouchers-rnd-labs/ https://innovationisrael.org.il/en/calls_for_proposal/usage-vouchers-rnd-labs/
https://arxiv.org/abs/2303.04137 https://arxiv.org/abs/2303.04137
https://dreamitate.cs.columbia.edu/ https://dreamitate.cs.columbia.edu/
https://research.nvidia.com/publication/2025-03_nvidia-isaac-gr00t-n1-open-foundation-model-humanoid-robots https://research.nvidia.com/publication/2025-03_nvidia-isaac-gr00t-n1-open-foundation-model-humanoid-robots
https://research.nvidia.com/labs/gear/dreamgen/ https://research.nvidia.com/labs/gear/dreamgen/
86
87
88
89
90
91
92
93
95
96
97
100
101
102
104
105
106