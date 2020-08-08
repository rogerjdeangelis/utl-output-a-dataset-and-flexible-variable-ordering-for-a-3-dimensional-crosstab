# utl-output-a-dataset-and-flexible-variable-ordering-for-a-3-dimensional-crosstab
Output dataset and flexible class variable ordering for a 3 dimensional crosstab
    SAS-L: Output dataset and flexible class variable ordering for a 3 dimensional crosstab                                                                     
                                                                                                                                                                
    Instead of a static proc tabulate report this method produces an output SAS dataset.                                                                        
    The rows are in correct order but you need to rearrange the columns and create.                                                                             
    You will need to create the partial sums or add 'all' levels to tje input data.                                                                             
                                                                                                                                                                
    Generally I find it a lot more useful to have a outout dataset that is close to the proc tabulate static output.                                            
                                                                                                                                                                
    Input provided by                                                                                                                                           
    data _null_,                                                                                                                                                
    datanull@gmail.com                                                                                                                                          
                                                                                                                                                                
    github                                                                                                                                                      
    https://tinyurl.com/y379cwvz                                                                                                                                
    https://github.com/rogerjdeangelis/utl-output-a-dataset-and-flexible-variable-ordering-for-a-3-dimensional-crosstab                                         
                                                                                                                                                                
    SAS-L                                                                                                                                                       
    https://listserv.uga.edu/cgi-bin/wa?A2=SAS-L;74d617ab.2008b                                                                                                 
                                                                                                                                                                
                                                                                                                                                                
    /*                   _                                                                                                                                      
    (_)_ __  _ __  _   _| |_                                                                                                                                    
    | | `_ \| `_ \| | | | __|                                                                                                                                   
    | | | | | |_) | |_| | |_                                                                                                                                    
    |_|_| |_| .__/ \__,_|\__|                                                                                                                                   
            |_|                                                                                                                                                 
    */                                                                                                                                                          
                                                                                                                                                                
    data have (keep = relation severity serious) ;                                                                                                              
       length rel1-rel7 sev1-sev4 ser1-ser3 $ 20 ;                                                                                                              
       array rel(*) $ rel1-rel7 ( 'Definite', 'Probable' , 'Possible' , 'Unlikely' , 'Not Related', 'Not Applicable' , 'Unrecorded') ;                          
       array sev(*) $ sev1-sev4 ('Mild' , 'Moderate' , 'Severe' , 'Unrecorded') ;                                                                               
       array ser(*) $ ser1-ser3 ('Yes' , 'No' , 'Unrecorded') ;                                                                                                 
       seed = 20200808 ;                                                                                                                                        
       do i = 1 to 100 ;                                                                                                                                        
          Relation = rel(ceil(ranuni(seed)* 7 )) ;                                                                                                              
          severity = sev(ceil(ranuni(seed)* 4 )) ;                                                                                                              
          serious = ser(ceil(ranuni(seed)* 3 )) ;                                                                                                               
          output ;                                                                                                                                              
       end ;                                                                                                                                                    
    run;                                                                                                                                                        
                                                                                                                                                                
    WORK.HAVE total obs=100                                                                                                                                     
                                                                                                                                                                
     RELATION          SEVERITY      SERIOUS                                                                                                                    
                                                                                                                                                                
     Possible          Unrecorded    Yes                                                                                                                        
     Probable          Severe        No                                                                                                                         
     Definite          Moderate      No                                                                                                                         
     Definite          Mild          Unrecorded                                                                                                                 
     Not Related       Severe        Yes                                                                                                                        
     Probable          Moderate      Yes                                                                                                                        
     Unlikely          Unrecorded    No                                                                                                                         
     Not Related       Moderate      No                                                                                                                         
     Possible          Severe        Unrecorded                                                                                                                 
     Possible          Severe        Unrecorded                                                                                                                 
     Definite          Severe        Yes                                                                                                                        
    ...                                                                                                                                                         
    ...                                                                                                                                                         
                                                                                                                                                                
    /*           _               _                                                                                                                              
      ___  _   _| |_ _ __  _   _| |_                                                                                                                            
     / _ \| | | | __| `_ \| | | | __|                                                                                                                           
    | (_) | |_| | |_| |_) | |_| | |_                                                                                                                            
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                                                           
                    |_|                                                                                                                                         
    */                                                                                                                                                          
                                                                                                                                                                
    WORK.WANT_COR total obs=8                                                                                                                                   
                                                                                                                                                                
                      NO___    NO___    NO___     NO___    UNRECORDED___  UNRECORDED___  UNRECORDED___  UNRECORDED___  YES___   YES___   YES___    YES___       
     LABEL             MILD  MODERATE  SEVERE  UNRECORDED       MILD         MODERATE        SEVERE       UNRECORDED    MILD   MODERATE  SEVERE  UNRECORDED  SUM
                                                                                                                                                                
     bDefinite          3        1        1         2             3             1              0              0           2        1        2         2       18
     cProbable          1        0        4         0             1             1              1              0           2        2        0         0       12
     dPossible          2        2        0         3             1             2              2              1           2        2        3         1       21
     eUnlikely          2        2        2         4             0             1              2              2           0        0        0         1       16
     fNot Related       0        3        0         1             1             1              2              1           0        0        1         3       13
     gNot Applicable    1        2        1         0             2             1              0              0           1        1        1         1       11
     hUnrecorded        0        1        1         3             2             0              1              0           0        0        1         0        9
     Sum                9       11        9        13            10             7              8              4           7        6        8         8      100
                                                                                                                                                                
    /*         _       _   _                                                                                                                                    
     ___  ___ | |_   _| |_(_) ___  _ __                                                                                                                         
    / __|/ _ \| | | | | __| |/ _ \| `_ \                                                                                                                        
    \__ \ (_) | | |_| | |_| | (_) | | | |                                                                                                                       
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                                                                       
                                                                                                                                                                
    */                                                                                                                                                          
    data have (keep = relation severity serious) ;                                                                                                              
       length rel1-rel7 sev1-sev4 ser1-ser3 $ 20 ;                                                                                                              
       array rel(*) $ rel1-rel7 ( 'Definite', 'Probable' , 'Possible' , 'Unlikely' , 'Not Related', 'Not Applicable' , 'Unrecorded') ;                          
       array sev(*) $ sev1-sev4 ('Mild' , 'Moderate' , 'Severe' , 'Unrecorded') ;                                                                               
       array ser(*) $ ser1-ser3 ('Yes' , 'No' , 'Unrecorded') ;                                                                                                 
       seed = 20200808 ;                                                                                                                                        
       do i = 1 to 100 ;                                                                                                                                        
          Relation = rel(ceil(ranuni(seed)* 7 )) ;                                                                                                              
          severity = sev(ceil(ranuni(seed)* 4 )) ;                                                                                                              
          serious = ser(ceil(ranuni(seed)* 3 )) ;                                                                                                               
          output ;                                                                                                                                              
       end ;                                                                                                                                                    
    run;                                                                                                                                                        
                                                                                                                                                                
    proc format ;                                                                                                                                               
      value $rel2Odr                                                                                                                                            
    "Definite      "=  "bDefinite      "                                                                                                                        
    "Probable      "=  "cProbable      "                                                                                                                        
    "Possible      "=  "dPossible      "                                                                                                                        
    "Unlikely      "=  "eUnlikely      "                                                                                                                        
    "Not Related   "=  "fNot Related   "                                                                                                                        
    "Not Applicable"=  "gNot Applicable"                                                                                                                        
    "Unrecorded    "=  "hUnrecorded    "                                                                                                                        
     ;run;quit;                                                                                                                                                 
                                                                                                                                                                
    ods exclude all;                                                                                                                                            
    ods output observed=want_cor;                                                                                                                               
    proc corresp data=play cross=both all dim=1 ;                                                                                                               
    format relation $rel2Odr.;                                                                                                                                  
    tables relation, serious severity;                                                                                                                          
    run;quit;                                                                                                                                                   
    ods select all;                                                                                                                                             
