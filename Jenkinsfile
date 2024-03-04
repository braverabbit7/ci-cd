pipeline {
    agent any

    stages {
        stage('First_stage') {
            steps {
                sh '''
                    mysql --user=rfamro --host=mysql-rfam-public.ebi.ac.uk --port=4497 --database=Rfam --execute="SELECT * FROM rfamseq LIMIT 1" | column -t > /tmp/first2.txt
                '''
            }
        }
        stage('Second_stage') {
            steps {
                sh '''
                   mysql --user rfamro --host mysql-rfam-public.ebi.ac.uk --port 4497 --database Rfam <<EOF > /tmp/second.txt
SELECT fr.rfam_acc, fr.rfamseq_acc, fr.seq_start, fr.seq_end
FROM full_region fr, rfamseq rf, taxonomy tx
WHERE rf.ncbi_id = tx.ncbi_id
AND fr.rfamseq_acc = rf.rfamseq_acc
AND tx.ncbi_id = 10116 -- NCBI taxonomy id of Rattus norvegicus
AND is_significant = 1 -- exclude low-scoring matches from the same clan
EOF
                '''
            }
        }
    }
}