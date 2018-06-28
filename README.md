# 15-Days-of-Learning-SQL
My answer of the "15 Days of Learning SQL" on HackerRank


select tabP.submission_date, tabP.hack_id_cnt, tabQ.first_id, hackers.name </br>
from
(select tabC.submission_date, count(tabC.hacker_id) as hack_id_cnt
from
(select tabA.submission_date, tabA.hacker_id, tabA.dailyFreq
from
(select submission_date, hacker_id, count(hacker_id) as dailyFreq from submissions
group by submission_date, hacker_id) as tabA
left join
(select submission_date, hacker_id, count(hacker_id) as dailyFreq from submissions
group by submission_date, hacker_id) as tabB
on tabA.hacker_id = tabB.hacker_id and tabA.submission_date >= tabB.submission_date
group by tabA.submission_date, tabA.hacker_id, tabA.dailyFreq
having day(tabA.submission_date) = count(tabB.submission_date)) as tabC
group by tabC.submission_date) as tabP
left join
(select tabG.submission_date, min(tabG.hacker_id) as first_id
from
(select submission_date, hacker_id, count(hacker_id) as hack_cnt
from submissions
group by submission_date, hacker_id) as tabG
inner join
(select submission_date, max(hack_cnt) as max_cnt
from
(
select submission_date, hacker_id, count(hacker_id) as hack_cnt
from submissions
group by submission_date, hacker_id
) as tabF
group by submission_date) as tabH
on tabG.submission_date = tabH.submission_date and tabG.hack_cnt = tabH.max_cnt
group by tabG.submission_date) as tabQ
using(submission_date)
left join
hackers
on tabQ.first_id = hackers.hacker_id
order by tabP.submission_date

