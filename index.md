---
layout: default
title: SOA Exam PA Notes
description: Open-source R notes for the Society of Actuaries Predictive Analytics exam.
samwiki: true
---

<section class="sw-lede" aria-labelledby="sw-about-title">
  <div class="sw-lede-copy">
    <h2 id="sw-about-title">Exam PA, worked in the open</h2>
    <p>These notes are an open-source record of preparing for the Society of Actuaries Predictive Analytics exam, written for the June 2019 sitting. The written notes are a <a href="{{ '/Exam%20PA%20Notes%20-%206-30-2019.pdf' | relative_url }}">PDF</a> and a <a href="{{ '/Exam%20PA%20Notes%20-%206-30-2019.onepkg' | relative_url }}">OneNote package</a> dated June 30, 2019, covering exam content, strategy, modeling methods, and code snippets. Exam PA was a five-hour computer-based project. A candidate received a business problem and a data set, then had to explore the data, fit and compare models in R, and write a report a non-modeler could use. The modules behind that sitting covered data types and graphics, principal components and clustering, generalized linear models, trees and random forests, and how to validate a model and explain it. The notes come from a first attempt that scored 7, about 100 hours of study, taken in the same window as a first sitting of STAM.</p>
    <p>The code uses the exam stack. dplyr and ggplot2 handle cleaning and graphics. caret builds stratified train and test splits. rpart fits trees. glm, and glmnet on the student-success solution, fit generalized linear models. Factor coding is part of the model. Reference levels move to the most common category so the intercept is a real baseline. Thin levels are collapsed before they become dummy variables. caret::dummyVars is run at full rank when a factor is expanded, so the design matrix does not pick up a redundant column.</p>
    <p>Hospital readmissions is a binary classification. The target is Readmission.Status. Three submissions clean length of stay, age, emergency-room use, and HCC risk score, then fit binomial GLMs. The last submission compares logit, probit, cauchit, and complementary log-log links, with a Gender-by-Race interaction as a worked example. Discrimination is read from an ROC curve and AUC. A later task prices errors at a probability cutoff of 0.075 and builds a confusion matrix, which is the right comparison when a missed readmission costs more than a false alarm.</p>
    <p>The December 2018 Miners Union practice predicts injury counts, with employee hours as exposure. That sitting used a less structured project format than June 2019. Poisson trees go through rpart’s poisson method, with cbind(EMP_HRS_TOTAL/2000, NUM_INJURIES) on the left-hand side, then pruning on cross-validated error. The Poisson GLM uses a log link and an offset of log(hours/2000), so the coefficients are log injury rates and a response-scale prediction is an expected count. Fit is scored with RMSE, MAE, and a Poisson loglikelihood that replaces nonpositive tree predictions before the log is taken.</p>
    <p>Student academic success is the first form of the June 2019 sample project, before the report structure was tightened. About 585 students are flagged pass or fail from final grade G3, with pass at 10 or above. Period grades G1 and G2, and absences, are dropped so they cannot leak the outcome. The comparison is a classification tree, a caret random forest, and a binomial logit GLM. The solution file also fits an L1-penalized logistic regression in glmnet (alpha = 1), with the penalty chosen by cross-validation. The syllabus has moved since 2019. Check the <a href="https://www.soa.org/education/exam-req/edu-exam-pa-detail/">SOA Exam PA page</a> before using these notes for a current sitting.</p>
  </div>
  <aside class="sw-find" aria-labelledby="sw-find-title">
    <h2 id="sw-find-title">In this repository</h2>
    <ul>
      <li><strong><a href="{{ '/Exam%20PA%20Notes%20-%206-30-2019.pdf' | relative_url }}">Written notes</a></strong> PDF from June 30, 2019, on exam content, strategy, modeling methods, and code snippets. The same notes are in a <a href="{{ '/Exam%20PA%20Notes%20-%206-30-2019.onepkg' | relative_url }}">OneNote package</a>.</li>
      <li><strong><a href="https://github.com/sdcastillo/SOA-PA-Exam/tree/master/Hospital%20Readmissions">Hospital readmissions</a></strong> Three submissions on a binary readmission flag, link functions, ROC, and a cost cutoff.</li>
      <li><strong><a href="https://github.com/sdcastillo/SOA-PA-Exam/tree/master/Miners%20Union">Miners Union</a></strong> December 2018 practice. Poisson tree and Poisson GLM with an exposure offset.</li>
      <li><strong><a href="https://github.com/sdcastillo/SOA-PA-Exam/tree/master/Student%20Academic%20Success">Student academic success</a></strong> June 2019 sample. Tree, random forest, logit GLM, and penalized logistic regression.</li>
      <li><strong>Factor helpers</strong> Relevel to the modal category, collapse rare levels, and dummy-code at full rank.</li>
      <li><strong>What the exam asked for</strong> A written recommendation, not only a model object. Validation and the business cutoff belong in the report.</li>
    </ul>
  </aside>
</section>

<section class="sw-videos" aria-labelledby="sw-videos-title">
  <h2 id="sw-videos-title">Videos</h2>
  <p>Sam Castillo's Exam PA videos: study tips, a live webinar, and worked practice-exam solutions, including the hospital readmissions project in this repository.</p>
  <div style="position:relative;padding-top:56.25%;margin:1rem 0;">
    <iframe src="https://rumble.com/embed/v7dies4/" title="Practice Exam - Hospital Readmissions - Solution" style="position:absolute;inset:0;width:100%;height:100%;border:0;" allowfullscreen loading="lazy"></iframe>
  </div>
  <ul>
    <li><a href="https://rumble.com/v7forpm-practice-exam-hospital-readmissions-solution.html">Practice Exam - Hospital Readmissions - Solution</a> (Rumble)</li>
    <li><a href="https://rumble.com/v7forfo-predictive-analytics-exampa-practice-exam-2-video-solution-reupload.html">Predictive Analytics (ExamPA) Practice Exam 2 Video Solution (Reupload)</a> (Rumble)</li>
    <li><a href="https://rumble.com/v7foqki-bicycle-sharing-demand-prediction-tutorial-exam-solution-walkthrough.html">Bicycle Sharing Demand Prediction Tutorial - Exam Solution Walkthrough</a> (Rumble)</li>
    <li><a href="https://rumble.com/v7fosfw-live-webinar-exam-pa-machine-learning-secrets-you-cant-miss.html">Live Webinar: Exam PA Machine Learning Secrets You Can't Miss!</a> (Rumble)</li>
    <li><a href="https://www.youtube.com/watch?v=F2okL4a2YcM">Unleash Your Potential: Step-by-Step Study Tips for Exam PA (2021 Update)!</a> (YouTube)</li>
    <li><a href="https://www.youtube.com/watch?v=FE5-pKaBHrg">Unlock the Power of Cost Complexity Pruning with Ease!</a> (YouTube)</li>
    <li><a href="https://www.youtube.com/watch?v=xDJXuoM6ZCI">Mastering GLM Link Functions: A Comprehensive Guide</a> (YouTube)</li>
  </ul>
</section>
