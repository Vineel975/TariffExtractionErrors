-- Does the claim resolve to a BPSIID?
SELECT TOP 1 ms.BPSIID
FROM Claims c
JOIN MemberSI ms ON CAST(ms.MemberPolicyID AS VARCHAR(50)) = CAST(c.MemberPolicyID AS VARCHAR(50))
WHERE c.ID = <CLAIM_ID> AND ISNULL(ms.Deleted,0)=0
ORDER BY ms.ID DESC;
