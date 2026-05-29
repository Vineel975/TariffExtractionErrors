DECLARE @ClaimID VARCHAR(50) = '26051694447';   -- <-- your claim ID

-- STEP 1: claim -> MemberPolicyID
SELECT 'STEP 1: MemberPolicyID' AS Step,
       CAST(MemberPolicyID AS VARCHAR(50)) AS MemberPolicyID
FROM Claims WITH (NOLOCK)
WHERE CAST(ID AS VARCHAR(50)) = @ClaimID AND ISNULL(Deleted,0)=0;

-- STEP 2: MemberPolicyID -> BPSIID  (uses the MemberPolicyID from step 1)
SELECT TOP 1 'STEP 2: BPSIID' AS Step,
       CAST(ms.BPSIID AS VARCHAR(50)) AS BPSIID
FROM MemberSI ms WITH (NOLOCK)
JOIN Claims c WITH (NOLOCK) ON CAST(c.MemberPolicyID AS VARCHAR(50)) = CAST(ms.MemberPolicyID AS VARCHAR(50))
WHERE CAST(c.ID AS VARCHAR(50)) = @ClaimID AND ISNULL(ms.Deleted,0)=0
ORDER BY ms.ID DESC;

-- STEP 3: BPSIID -> what USP_BPSumInsured_Retrieve returns
-- (run this separately AFTER you get the BPSIID from step 2)
-- EXEC USP_BPSumInsured_Retrieve @BPSIID = <BPSIID_from_step2>;
