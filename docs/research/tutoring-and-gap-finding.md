# How good tutors check understanding and adapt to it

This file answers [issue 3](https://github.com/calmxz/Suro/issues/3). The question: how does a tutor find what a learner does not understand, and then teach to it?

It covers four parts of the question:

- How human tutors check understanding, and which question types reveal a gap.
- How existing AI tutors model what a learner knows.
- Review scheduling over days, and whether it matters for a v1.
- Which findings have strong evidence, and which are only common practice.

The file reports findings and trade-offs. It does not make the v1 decision. A later ticket makes that decision with the owner.

## Short answer

- Tutoring helps, but by far less than the famous "2 sigma" claim. Large reviews of randomized trials find about 0.3 standard deviations.
- "Do you understand?" is a poor check. Answers to it mislead tutors. The quality of the tutee's answers to real questions is a better signal.
- Prompts to explain and questions that require recall both reveal understanding and improve learning.
- Classic tutoring systems keep a per-concept estimate of what the tutee knows. Simple models do about as well as deep models.
- LLM tutors can help when they have guardrails. Without guardrails, they can harm learning. Their skill at diagnosis is still weak on published benchmarks.
- Spacing review over days has strong evidence for long-term recall. No source shows that a complex scheduler beats a simple one for understanding.

## How to read the labels

Each finding has one label.

- **Strong evidence**: meta-analyses or several randomized trials agree on the direction of the effect.
- **Mixed evidence**: the evidence is from one trial, small studies, observational data, or benchmarks, or the results conflict.
- **Common practice**: tutors or tutoring systems do this widely, but the sources here do not test whether it improves learning.

A note on terms. Many papers use words such as "mastery" and "gap" in their own meaning. This file marks those uses as the literature's terms. In this file, "mastery" and "gap" without a qualifier have their Suro glossary meaning.

This file calls the people in studies "tutees" or "participants." The word "learner" means an adult who uses Suro. "Learner model" is the literature's term for a model of what a tutee knows.

## 1. How much tutoring helps, and the "2 sigma" claim

### 1.1 Human tutoring helps by about 0.3 to 0.8 standard deviations

**Label: strong evidence.**

- A meta-analysis of randomized field experiments of preK-12 tutoring finds a pooled effect of 0.288 SD (SE 0.029) ([Nickow, Oreopoulos, and Quan, 2024](https://doi.org/10.3102/00028312231208687)).
- Effects are largest when teachers or paraprofessionals tutor, in earlier grades, at 3 or more days a week, and during school hours (same source).
- A review of experiments that compare tutoring with no tutoring finds d = 0.79 for human tutoring ([VanLehn, 2011](https://doi.org/10.1080/00461520.2011.611369)).
- The same review finds d = 0.76 for intelligent tutoring systems. VanLehn concludes that they are nearly as effective as human tutors.

### 1.2 The "2 sigma" figure is contested

**Label: mixed evidence.**

- [Bloom (1984)](https://doi.org/10.3102/0013189X013006004) bases the 2 sigma claim on two doctoral dissertations by his own doctoral advisees, Anania and Burke.
- In those studies, the tutoring condition also included formative tests, feedback-corrective procedures, and parallel retests (Bloom, 1984).
- VanLehn (2011) states that the belief in a 2.0 effect for human tutors is not confirmed by his review.
- [von Hippel (2024)](https://www.educationnext.org/wp-content/uploads/2024/03/ednext_XIV_2_von_hippel.pdf) reads the two dissertations. *Education Next* is a magazine, not a peer-reviewed journal.
- He reports that the experiments lasted three weeks, used narrow tests, and used topics that were new to the participants.
- He reports that the literature's "mastery learning" group in those studies, with testing and feedback but no tutor, scored about 1.1 SD above whole-class teaching.
- He also reports that a 1982 meta-analysis by Cohen, Kulik, and Kulik found about 0.33 SD for tutoring.
- He reports that none of the 96 studies in the 2020 Nickow working paper reached 2 SD.

### 1.3 Intelligent tutoring systems help, and the test type changes the size

**Label: strong evidence.**

- A meta-analysis of 50 controlled evaluations finds a median effect of 0.66 SD over conventional teaching ([Kulik and Fletcher, 2016](https://doi.org/10.3102/0034654315581420)).
- Kulik and Fletcher add that effects depend strongly on whether the test was locally developed or standardized.
- A second meta-analysis of 107 effect sizes finds g = 0.42 over teacher-led, large-group teaching ([Ma, Adesope, Nesbit, and Liu, 2014](https://doi.org/10.1037/a0037123)).
- That meta-analysis finds no significant difference between these systems and individual human tutoring (g = -0.11).

### 1.4 How to read effect sizes in this file

**Label: strong evidence.**

- [Kraft (2020)](https://doi.org/10.3102/0013189X20912798) proposes benchmarks from 1,942 effect sizes in 747 randomized trials with standardized test outcomes.
- His benchmarks are: below 0.05 is small, 0.05 to below 0.20 is medium, and 0.20 or more is large. The median is 0.10 SD.
- These benchmarks apply to preK-12 interventions with standardized tests.
- Many studies in this file use narrow tests made for the study. Kulik and Fletcher (2016) show that the test type strongly changes effect sizes.

## 2. How human tutors check understanding

### 2.1 Tutors mostly ask "Do you understand?", and the answers mislead

**Label: mixed evidence.** The direction is consistent, but the studies are small and observational.

- [Graesser, Person, and Magliano (1995)](https://doi.org/10.1002/acp.2350090604) describe a common 5-step dialogue frame in tutoring:
  1. The tutor asks a question.
  2. The tutee answers.
  3. The tutor gives short feedback.
  4. The tutor and tutee improve the answer together.
  5. The tutor checks the tutee's understanding of the answer.
- At step 5, tutors simply asked "Do you understand?" or a similar question 92% of the time. They asked a follow-up question that tested understanding only 7% of the time.
- The authors call tutee answers to these comprehension-gauging questions "notoriously inaccurate."
- [Person, Graesser, Magliano, and Kreuz (1994)](https://doi.org/10.1016/1041-6080(94)90010-8) analyze 44 tutoring sessions with undergraduates in research methods.
- They find that the quality of tutee answers is the most reliable signal of understanding.
- They find that answers to "Do you understand?" are "very misleading."
- Higher achievers said "No, I do not understand" more often. "Yes" answers were most common among average achievers, at 0.62 and 0.61, and lower among top achievers, at 0.46. This pattern was only marginally significant (p < .10).
- The authors advise tutors to be suspicious of tutees who rarely say they do not understand.

### 2.2 People misjudge their own learning, and the error costs them

**Label: mixed evidence.** Two experiments and one field trial agree.

- In two studies with college participants, more accurate self-judgments went with better recall two days later ([Dunlosky and Rawson, 2012](https://doi.org/10.1016/j.learninstruc.2011.08.003)).
- A field trial gave high school participants a chatbot ([Bastani and others, 2025](https://doi.org/10.1073/pnas.2422633122)).
- Those with a plain chatbot scored worse on the exam, but did not perceive that they learned less.
- Those who used the guarded tutor did not score better on the exam, but perceived that they did (same source).

### 2.3 Human tutors are poor at diagnosing misconceptions

**Label: mixed evidence.** One recoded data set.

- [Chi, Siler, and Jeong (2004)](https://doi.org/10.1207/s1532690xci2203_4) find that tutors could assess what a tutee knew of the correct model.
- The same study finds that tutors were "dismal" at diagnosing a tutee's alternative, incorrect understanding.

### 2.4 Prompting the tutee can work as well as explaining to them

**Label: mixed evidence.** One controlled study.

- In [Chi, Siler, Jeong, and others (2001)](https://doi.org/10.1207/s15516709cog2504_1), one group of tutors was not allowed to give explanations or feedback. They could only prompt.
- Participants learned just as effectively in that interactive style. The authors relate this to deeper scaffolding and more tutee effort.

### 2.5 Tutors use a fixed question-answer-feedback cycle

**Label: common practice.**

- The 5-step frame in section 2.1 is a description of what ordinary, untrained tutors do. Graesser and others (1995) do not test it as a teaching method.

## 3. Which question types reveal a gap

### 3.1 Asking the tutee to explain both reveals and builds understanding

**Label: strong evidence** for the learning effect. The diagnostic value is mixed evidence.

- [Chi, De Leeuw, Chiu, and LaVancher (1994)](https://doi.org/10.1207/s15516709cog1803_3) asked 14 eighth graders to self-explain after each line of a text. Ten controls read the text twice.
- The prompted group gained more. All high explainers reached the correct mental model, and many others did not.
- The explanations also showed the researchers which mental model each participant held.
- A meta-analysis covers 69 effect sizes and 5,917 participants ([Bisra, Liu, Nesbit, Salimi, and Winne, 2018](https://doi.org/10.1007/s10648-018-9434-x)).
- It finds g = 0.55 (95% CI 0.45 to 0.65) for self-explanation prompts.
- [Dunlosky and others (2013)](https://doi.org/10.1177/1529100612453266) rate self-explanation "moderate utility." Their reason is limited evidence in real educational settings.

### 3.2 Cued recall questions teach more than recognition questions

**Label: strong evidence.**

- A meta-analysis of testing versus restudy finds g = 0.50 (95% CI 0.42 to 0.58) in favor of testing ([Rowland, 2014](https://doi.org/10.1037/a0037559)).
- In that meta-analysis, cued recall practice tests give g = 0.61. Free recall and recognition tests both give g = 0.29.
- A second meta-analysis finds that practice tests beat restudy and every other comparison condition ([Adesope, Trevisan, and Sundararajan, 2017](https://doi.org/10.3102/0034654316689306)).
- Dunlosky and others (2013) rate practice testing "high utility."
- This evidence is about retention. It does not show which question type best reveals a gap.

### 3.3 Engagement that makes the tutee produce something works better

**Label: mixed evidence.** This is a framework with supporting studies.

- The ICAP framework orders learning activities as passive, active, constructive, and interactive ([Chi and Wylie, 2014](https://doi.org/10.1080/00461520.2014.965823)).
- It predicts that learning increases in that order. Self-explaining is one of its constructive examples.

### 3.4 Diagnostic multiple-choice questions map wrong answers to misconceptions

**Label: common practice.**

- In diagnostic questions, each wrong option points to a specific misconception ([Wang and others, 2020](https://arxiv.org/abs/2007.12061)).
- The Eedi platform uses these questions in mathematics. Its data set has more than 20 million answers (same source).
- This source describes a data set and a competition. It does not test whether these questions improve learning.

## 4. Teaching to the gap

### 4.1 Feedback helps on average, but a large share of feedback hurts

**Label: mixed evidence.**

- A meta-analysis of 607 effect sizes finds an average feedback effect of d = 0.41 ([Kluger and DeNisi, 1996](https://doi.org/10.1037/0033-2909.119.2.254)).
- More than one third of the feedback interventions lowered performance.
- Feedback works less well as it moves attention away from the task and toward the self.

### 4.2 Large claims for formative assessment do not hold up

**Label: mixed evidence.**

- An effect of 0.4 to 0.7 is often claimed for formative assessment ([Kingston and Nash, 2011](https://doi.org/10.1111/j.1745-3992.2011.00220.x)).
- Kingston and Nash found only 13 usable studies out of more than 300. The weighted mean effect is 0.20.
- Computer-based formative systems gave 0.28 in their moderator analysis.
- [Bennett (2011)](https://doi.org/10.1080/0969594X.2010.513678) calls the common quantitative claims "suspect." He traces them to flawed, dated, or unpublished work.

### 4.3 The literature's "mastery learning" helps, with a cost in time

**Label: strong evidence.**

- In this literature, "mastery learning" means testing, correcting, and retesting until a tutee passes a set level. This differs from Suro's meaning of mastery.
- A meta-analysis of 108 controlled evaluations finds positive effects on exam performance ([Kulik, Kulik, and Bangert-Drowns, 1990](https://doi.org/10.3102/00346543060002265)).
- The effects are stronger for weaker participants in a class.
- These programs can increase time on task. Self-paced versions often reduce completion rates in college classes.

### 4.4 Do not give away the answer

**Label: mixed evidence.** One field trial, one trial with human tutors, and one data set agree.

- Bastani and others (2025) find that guarded tutor prompts with teacher-designed hints mostly removed the harm of a plain chatbot.
- With an AI assistant for human tutors, tutors asked more guiding questions and gave away fewer answers ([Wang, Ribeiro, Robinson, Loeb, and Demszky, 2024](https://arxiv.org/abs/2410.03017)).
- [Macina and others (2023)](https://arxiv.org/abs/2305.14536) report that models such as GPT-3 solve problems well, but tutor poorly. They reveal solutions too early and give incorrect feedback.

## 5. How existing AI tutors model what a tutee knows

### 5.1 Classic tutoring systems keep a per-concept estimate

**Label: common practice.**

- Bayesian knowledge tracing (BKT) comes from [Corbett and Anderson (1994)](https://doi.org/10.1007/BF01099821). The description below comes from Pelanek's overview.
- BKT treats each knowledge component, or concept, as known or unknown ([Pelanek, 2017](https://doi.org/10.1007/s11257-017-9193-2)).
- It has four parameters per concept: initial knowledge, learning rate, slip, and guess.
- A slip is a wrong answer when the concept is known. A guess is a right answer when it is unknown.
- Logistic models, such as performance factor analysis and the Elo rating system, are the other main family (Pelanek, 2017).

### 5.2 Deep models do not clearly beat simple models

**Label: mixed evidence.**

- Deep knowledge tracing uses recurrent neural networks. Its authors report substantial gains in prediction ([Piech and others, 2015](https://arxiv.org/abs/1506.05908)).
- [Khajah, Lindsey, and Mozer (2016)](https://arxiv.org/abs/1604.02416) extend BKT to capture four regularities, including recency effects and individual variation in ability.
- With those extensions, BKT performs indistinguishably from deep knowledge tracing.
- Pelanek (2017) states that, for decisions on when a concept is learned, the input data matter more than the model details.
- He adds that slightly different models with the same input data lead to very similar decisions of that kind.

### 5.3 LLMs as the literature's learner models are early and not yet better

**Label: mixed evidence.**

- Fine-tuned LLMs perform on par with standard BKT, but do not reach the best existing models ([Neshaei and others, 2024](https://arxiv.org/abs/2403.14661)).
- An algebra benchmark has 55 misconceptions and 220 diagnostic examples. An LLM's accuracy varies by topic and setup ([Otero, Druga, and Lan, 2024](https://arxiv.org/abs/2412.03765)).
- That accuracy reaches 83.9% only with educator feedback and with the task limited by topic.

### 5.4 LLM tutors are weak at diagnosis on current benchmarks

**Label: mixed evidence.** These are benchmarks, not learning outcomes.

- On TutorBench, no frontier LLM scores above 56% ([Srinivasa and others, 2025](https://arxiv.org/abs/2510.02663)).
- The authors find that LLMs fall short of the full range of skills to guide, diagnose, and support the people they tutor.
- On MathTutorBench, skill at solving does not translate into skill at teaching ([Macina and others, 2025](https://arxiv.org/abs/2502.18940)).
- Tutoring gets harder in longer dialogs, where simple questioning strategies begin to fail (same source).
- LearnLM treats pedagogy as instruction following. Experts prefer it to other models across learning scenarios ([LearnLM Team, 2024](https://arxiv.org/abs/2412.16429)).

## 6. Evaluations of LLM tutors

### 6.1 A structured AI tutor beat an active-learning class in one course

**Label: mixed evidence.** One trial in one course.

- [Kestin and others (2025)](https://doi.org/10.1038/s41598-025-97652-6) ran a crossover trial with 194 participants in an undergraduate physics course at Harvard.
- The comparison is an in-class active-learning session, not a human tutor.
- Median learning gains in the AI group were more than double. The linear regression gives an effect of 0.63. A quantile regression gives 0.73 to 1.3 SD.
- The post-test came right after the session. The study does not measure retention over days.
- The authors found that a system prompt alone could not reliably sequence multi-part problems. The platform guided participants through each part in order.
- They added full step-by-step answers to the prompts to reduce wrong explanations.

### 6.2 A plain chatbot can harm learning; a guarded tutor removes most of the harm

**Label: mixed evidence.** One large field trial.

- Bastani and others (2025) gave nearly a thousand high school participants one of two GPT-4 tutors.
- During practice, grades rose 48% with the plain chatbot and 127% with the guarded tutor.
- On the later exam without AI, the plain chatbot group scored 17% lower than the control group.
- The guarded tutor group did not score better than the control group on the exam. The safeguards mostly removed the harm.

### 6.3 Other trials report moderate gains

**Label: mixed evidence.**

- In Nigeria, a six-week program used Microsoft Copilot with GPT-4 for English ([De Simone and others, 2025](https://ideas.repec.org/p/wbk/wbrwps/11125.html)).
- It finds 0.31 SD on the overall assessment and 0.23 SD on English. This is a World Bank working paper.
- An AI assistant for human tutors raised the literature's "topic mastery" by 4 percentage points (Wang and others, 2024).
- Participants of lower-rated tutors gained 9 percentage points. The tool costs about $20 per tutor per year (same source).

## 7. Review scheduling over days

### 7.1 Spacing review improves long-term recall

**Label: strong evidence.** Most of this evidence is about recall of facts and word lists.

- A synthesis covers 839 assessments in 317 experiments ([Cepeda and others, 2006](https://doi.org/10.1037/0033-2909.132.3.354)).
- It finds that the best interval between study sessions grows as the retention interval grows.
- [Cepeda and others (2008)](https://doi.org/10.1111/j.1467-9280.2008.02209.x) taught facts to more than 1,350 participants. They tested intervals between sessions up to 3.5 months and final tests up to 1 year later.
- For a test 1 week later, the best interval between sessions was about 20 to 40% of that week.
- For a test 1 year later, the best interval was about 5 to 10% of that year.
- Dunlosky and others (2013) rate distributed practice "high utility."

### 7.2 Algorithmic schedulers exist, but the evidence is about recall prediction and engagement

**Label: mixed evidence.**

- Half-life regression at Duolingo reduces error in predicting recall by 45% or more ([Settles and Meeder, 2016](https://doi.org/10.18653/v1/P16-1174)).
- In an operational study at Duolingo, it raised daily engagement by 12%. This is engagement, not learning.
- A Duolingo natural experiment finds that people who follow the MEMORIZE schedule memorize more effectively than people who follow heuristic schedules ([Tabibian and others, 2019](https://doi.org/10.1073/pnas.1815156116)).
- A scheduler trained on 220 million memory logs reports a 12.6% improvement over earlier methods on its own metric ([Ye, Su, and Cao, 2022](https://doi.org/10.1145/3534678.3539081)).
- Anki offers the Free Spaced Repetition Scheduler (FSRS) as an alternative to its older SM-2 algorithm ([Anki manual](https://docs.ankiweb.net/deck-options.html)). This is common practice.
- All of these schedulers target recall of items such as words or flashcards. No source here tests them on understanding of concepts.

### 7.3 Whether scheduling matters for a v1

**Label: mixed evidence.** This section states trade-offs, not a decision. Except for the Kestin point, these bullets are reasoning from sections 7.1 and 7.2, not findings.

- Spacing helps only when a learner comes back on a later day. The v1 testers are the owner and volunteers, and no source here predicts their return rate.
- Kestin and others (2025) list spacing as future work. Their trial measured learning on one day only.
- A simple option is a fixed interval that grows after each successful review. The sources here do not compare it with a trained scheduler.
- The need to bring a learner back can mean reminders, such as email. That has its own cost and privacy questions.

## 8. Limits of this evidence

- Almost every study here is with children, teenagers, or undergraduates. Suro's learners are adults.
- Almost every study uses a fixed curriculum chosen by researchers or teachers. No study here tests a tutor that teaches a material the learner chose.
- Kraft's benchmarks apply to preK-12 interventions with standardized tests.
- Several LLM results are preprints, working papers, or benchmarks. Only Kestin and others (2025) and Bastani and others (2025) are peer-reviewed trials of LLM tutors in this file.
- LLM models change fast. Results with GPT-3 or GPT-4 may not hold for current models.

## What this means for Suro

These points connect the findings to Suro. They do not make the v1 decision.

- If Suro asks "Do you understand?", the answer is weak evidence of mastery. The evidence supports asking the learner to explain or apply the concept, which matches Suro's glossary meaning of mastery.
- Open recall and explain questions serve two goals at once: they reveal a gap and they improve retention.
- Multiple-choice questions with wrong options mapped to misconceptions are common practice. They need known misconceptions, which a new material may not have.
- A tutor that gives away answers can harm learning. Guardrails and hints, not full solutions, are the pattern with evidence.
- A simple per-concept state, such as BKT or a logistic model, is well established. The model choice matters less than the quality of the input data.
- For Suro, the input data is an LLM's judgment of a free-text explanation. Current benchmarks show that LLM diagnosis is weak. This judgment is the main risk to test.
- Spacing has strong evidence for recall. Whether it matters for v1 depends on whether learners return on later days.

## Not verified

- The overall effect size in Adesope and others (2017). Secondary sources give different numbers. I read only the abstract, which gives no number.
- VanLehn's (2011) effect sizes for answer-based and substep-based systems. I verified only the 0.79 and 0.76 in the abstract.
- The effect size in Kulik, Kulik, and Bangert-Drowns (1990). The abstract gives no number.
- The text of Corbett and Anderson (1994). The BKT description comes from Pelanek (2017).
- The original 0.4 to 0.7 claim in Black and Wiliam (1998). I cite it only through Kingston and Nash (2011).
- The Cohen, Kulik, and Kulik (1982) 0.33 figure and the 2020 working-paper 0.37 figure. Both come from von Hippel (2024).
- Whether the LLM tutors in Kestin and others (2025) and Bastani and others (2025) keep a per-concept model of the tutee across sessions. I did not find this in the parts I read.
- How much review data a trained scheduler, such as FSRS, needs before it fits well.
- Any study of how accurately an LLM grades an adult's free-text explanation of a concept from a material.

## References

- Adesope, O. O., Trevisan, D. A., and Sundararajan, N. (2017). Rethinking the use of tests: A meta-analysis of practice testing. *Review of Educational Research*, 87(3), 659-701. https://doi.org/10.3102/0034654316689306
- Anki manual. Deck options: FSRS. https://docs.ankiweb.net/deck-options.html
- Bastani, H., Bastani, O., Sungu, A., and others (2025). Generative AI without guardrails can harm learning: Evidence from high school mathematics. *PNAS*, 122(26). https://doi.org/10.1073/pnas.2422633122
- Bennett, R. E. (2011). Formative assessment: A critical review. *Assessment in Education*, 18(1), 5-25. https://doi.org/10.1080/0969594X.2010.513678
- Bisra, K., Liu, Q., Nesbit, J. C., Salimi, F., and Winne, P. H. (2018). Inducing self-explanation: A meta-analysis. *Educational Psychology Review*, 30(3), 703-725. https://doi.org/10.1007/s10648-018-9434-x
- Bloom, B. S. (1984). The 2 sigma problem. *Educational Researcher*, 13(6), 4-16. https://doi.org/10.3102/0013189X013006004
- Cepeda, N. J., Pashler, H., Vul, E., and others (2006). Distributed practice in verbal recall tasks. *Psychological Bulletin*, 132(3), 354-380. https://doi.org/10.1037/0033-2909.132.3.354
- Cepeda, N. J., Vul, E., Rohrer, D., and others (2008). Spacing effects in learning: A temporal ridgeline of optimal retention. *Psychological Science*, 19(11), 1095-1102. https://doi.org/10.1111/j.1467-9280.2008.02209.x
- Chi, M. T. H., De Leeuw, N., Chiu, M.-H., and LaVancher, C. (1994). Eliciting self-explanations improves understanding. *Cognitive Science*, 18(3), 439-477. https://doi.org/10.1207/s15516709cog1803_3
- Chi, M. T. H., Siler, S. A., Jeong, H., and others (2001). Learning from human tutoring. *Cognitive Science*, 25(4), 471-533. https://doi.org/10.1207/s15516709cog2504_1
- Chi, M. T. H., Siler, S. A., and Jeong, H. (2004). Can tutors monitor students' understanding accurately? *Cognition and Instruction*, 22(3), 363-387. https://doi.org/10.1207/s1532690xci2203_4
- Chi, M. T. H., and Wylie, R. (2014). The ICAP framework. *Educational Psychologist*, 49(4), 219-243. https://doi.org/10.1080/00461520.2014.965823
- Corbett, A. T., and Anderson, J. R. (1994). Knowledge tracing: Modeling the acquisition of procedural knowledge. *User Modeling and User-Adapted Interaction*, 4(4), 253-278. https://doi.org/10.1007/BF01099821
- De Simone, M. E., Tiberti, F. H., Barron Rodriguez, M. R., and others (2025). From chalkboards to chatbots: Evaluating the impact of generative AI on learning outcomes in Nigeria. World Bank Policy Research Working Paper 11125. https://ideas.repec.org/p/wbk/wbrwps/11125.html
- Dunlosky, J., and Rawson, K. A. (2012). Overconfidence produces underachievement: Inaccurate self evaluations undermine students' learning and retention. *Learning and Instruction*, 22(4), 271-280. https://doi.org/10.1016/j.learninstruc.2011.08.003
- Dunlosky, J., Rawson, K. A., Marsh, E. J., Nathan, M. J., and Willingham, D. T. (2013). Improving students' learning with effective learning techniques. *Psychological Science in the Public Interest*, 14(1), 4-58. https://doi.org/10.1177/1529100612453266
- Graesser, A. C., Person, N. K., and Magliano, J. P. (1995). Collaborative dialogue patterns in naturalistic one-to-one tutoring. *Applied Cognitive Psychology*, 9(6), 495-522. https://doi.org/10.1002/acp.2350090604
- Khajah, M., Lindsey, R. V., and Mozer, M. C. (2016). How deep is knowledge tracing? https://arxiv.org/abs/1604.02416
- Kestin, G., Miller, K., Klales, A., Milbourne, T., and Ponti, G. (2025). AI tutoring outperforms in-class active learning. *Scientific Reports*, 15, 17458. https://doi.org/10.1038/s41598-025-97652-6
- Kingston, N., and Nash, B. (2011). Formative assessment: A meta-analysis and a call for research. *Educational Measurement: Issues and Practice*, 30(4), 28-37. https://doi.org/10.1111/j.1745-3992.2011.00220.x
- Kluger, A. N., and DeNisi, A. (1996). The effects of feedback interventions on performance. *Psychological Bulletin*, 119(2), 254-284. https://doi.org/10.1037/0033-2909.119.2.254
- Kraft, M. A. (2020). Interpreting effect sizes of education interventions. *Educational Researcher*, 49(4), 241-253. https://doi.org/10.3102/0013189X20912798
- Kulik, C.-L. C., Kulik, J. A., and Bangert-Drowns, R. L. (1990). Effectiveness of mastery learning programs: A meta-analysis. *Review of Educational Research*, 60(2), 265-299. https://doi.org/10.3102/00346543060002265
- Kulik, J. A., and Fletcher, J. D. (2016). Effectiveness of intelligent tutoring systems: A meta-analytic review. *Review of Educational Research*, 86(1), 42-78. https://doi.org/10.3102/0034654315581420
- LearnLM Team (2024). LearnLM: Improving Gemini for learning. https://arxiv.org/abs/2412.16429
- Ma, W., Adesope, O. O., Nesbit, J. C., and Liu, Q. (2014). Intelligent tutoring systems and learning outcomes: A meta-analysis. *Journal of Educational Psychology*, 106(4), 901-918. https://doi.org/10.1037/a0037123
- Macina, J., Daheim, N., Pal Chowdhury, S., and others (2023). MathDial: A dialogue tutoring dataset with rich pedagogical properties grounded in math reasoning problems. https://arxiv.org/abs/2305.14536
- Macina, J., Daheim, N., Hakimi, I., Kapur, M., and others (2025). MathTutorBench: A benchmark for measuring open-ended pedagogical capabilities of LLM tutors. https://arxiv.org/abs/2502.18940
- Neshaei, S. P., Davis, R. L., Hazimeh, A., Lazarevski, B., and others (2024). Towards modeling learner performance with large language models. https://arxiv.org/abs/2403.14661
- Nickow, A., Oreopoulos, P., and Quan, V. (2024). The promise of tutoring for preK-12 learning: A systematic review and meta-analysis of the experimental evidence. *American Educational Research Journal*. https://doi.org/10.3102/00028312231208687
- Otero, N., Druga, S., and Lan, A. (2024). A benchmark for math misconceptions: Bridging gaps in middle school algebra with AI-supported instruction. https://arxiv.org/abs/2412.03765
- Pelanek, R. (2017). Bayesian knowledge tracing, logistic models, and beyond: An overview of learner modeling techniques. *User Modeling and User-Adapted Interaction*, 27, 313-350. https://doi.org/10.1007/s11257-017-9193-2
- Person, N. K., Graesser, A. C., Magliano, J. P., and Kreuz, R. J. (1994). Inferring what the student knows in one-to-one tutoring: The role of student questions and answers. *Learning and Individual Differences*, 6(2), 205-229. https://doi.org/10.1016/1041-6080(94)90010-8
- Piech, C., Spencer, J., Huang, J., Ganguli, S., and others (2015). Deep knowledge tracing. https://arxiv.org/abs/1506.05908
- Rowland, C. A. (2014). The effect of testing versus restudy on retention: A meta-analytic review of the testing effect. *Psychological Bulletin*, 140(6), 1432-1463. https://doi.org/10.1037/a0037559
- Settles, B., and Meeder, B. (2016). A trainable spaced repetition model for language learning. *Proceedings of ACL 2016*, 1848-1858. https://doi.org/10.18653/v1/P16-1174
- Srinivasa, R. S., Che, Z., Zhang, C. B. C., Mares, D., and others (2025). TutorBench: A benchmark to assess tutoring capabilities of large language models. https://arxiv.org/abs/2510.02663
- Tabibian, B., Upadhyay, U., De, A., and others (2019). Enhancing human learning via spaced repetition optimization. *PNAS*, 116(10), 3988-3993. https://doi.org/10.1073/pnas.1815156116
- VanLehn, K. (2011). The relative effectiveness of human tutoring, intelligent tutoring systems, and other tutoring systems. *Educational Psychologist*, 46(4), 197-221. https://doi.org/10.1080/00461520.2011.611369
- von Hippel, P. T. (2024). Two-sigma tutoring: Separating science fiction from science fact. *Education Next*, Spring 2024. https://www.educationnext.org/wp-content/uploads/2024/03/ednext_XIV_2_von_hippel.pdf
- Wang, R. E., Ribeiro, A. T., Robinson, C. D., Loeb, S., and Demszky, D. (2024). Tutor CoPilot: A human-AI approach for scaling real-time expertise. https://arxiv.org/abs/2410.03017
- Wang, Z., Lamb, A., Saveliev, E., Cameron, P., and others (2020). Instructions and guide for diagnostic questions: The NeurIPS 2020 Education Challenge. https://arxiv.org/abs/2007.12061
- Ye, J., Su, J., and Cao, Y. (2022). A stochastic shortest path algorithm for optimizing spaced repetition scheduling. *Proceedings of KDD 2022*, 4381-4390. https://doi.org/10.1145/3534678.3539081
