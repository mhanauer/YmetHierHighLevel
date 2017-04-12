# YmetHierHighLevel
# Example for Jags-Ymet-XmetMulti-Mrobust.R 
#------------------------------------------------------------------------------- 
# Optional generic preliminaries:
graphics.off() # This closes all of R's graphics windows.
rm(list=ls())  # Careful! This clears all of R's memory!
# UNCOMMENT ONE OF THE FOLLOWING SECTIONS (In RStudio, select and ctrl-shift-C)
#.............................................................................
# # Two predictors:
setwd("~/Desktop/Extending")
myData = read.csv( file="HierLinRegressDicotDataExtend.csv" )
yName = "Y" ; xName = c("X","X2","X3"); sName="Subj"
fileNameRoot = "Jags-Ymet-XmetMulti-Mrobust-" 
numSavedSteps=15000 ; thinSteps=5
#.............................................................................

graphFileType = "eps" 
#------------------------------------------------------------------------------- 
# Load the relevant model into R's working memory:
source("Jags-Ymet-XmetMultiHier-Mrobust.R")
#------------------------------------------------------------------------------- 
# Generate the MCMC chain:
#startTime = proc.time()
mcmcCoda = genMCMC( data=myData , xName=xName , yName=yName , sName = sName, 
                    numSavedSteps=numSavedSteps , thinSteps=thinSteps , 
                    saveName=fileNameRoot )
#stopTime = proc.time()
#duration = stopTime - startTime
#show(duration)
#------------------------------------------------------------------------------- 
# Display diagnostics of chain, for specified parameters:
parameterNames = varnames(mcmcCoda) # get all parameter names
for ( parName in parameterNames ) {
  diagMCMC( codaObject=mcmcCoda , parName=parName , 
            saveName=fileNameRoot , saveType=graphFileType )
}
#------------------------------------------------------------------------------- 
# Get summary statistics of chain:
summaryInfo = smryMCMC( mcmcCoda , 
                        saveName=fileNameRoot )
show(summaryInfo)
# Display posterior information:
plotMCMC( mcmcCoda , data=myData , xName=xName , yName=yName , sName = sName, 
          pairsPlot=TRUE , showCurve=FALSE ,
          saveName=fileNameRoot , saveType=graphFileType )
#------------------------------------------------------------------------------- 
