

for(i in 1:nrow(region)){
  
  #progress 
  startTime <- Sys.time()
  print(paste(i,"Start",startTime))
  flush.console()
  
  infoFile <- fread(
    paste0("./ImputeCambridge1000GP_Phase3_mergedInfo/",
           region[i, "chrom"],
           ".info"),verbose = FALSE)
  setkeyv(infoFile,c("position"))
  #region of interest
  Chr=region[i, "chrom"]
  Start=region[i, "Start20Kb"]
  End=region[i, "End20Kb"]
  
  #subset region
  output <- infoFile[ infoFile$position >= Start &
                        infoFile$position <= End,]
  #add output region name as a column
  output <- cbind(region=paste(Chr,Start,End,sep = "_"),
                  output)
  #write with append to have 1 file for all chromosomes
  write.table(output,"20150505_DRG.info",
              row.names=FALSE,
              col.names=(i==1),
              quote=FALSE,append=TRUE)
  
  #progress 
  endTime <- Sys.time()
  print(paste(i,"End", endTime,
              " | Duration:", 
              #duration in minutes
              round((as.numeric(endTime)-as.numeric(startTime))/60,2)
  ))
  flush.console()
  
  } #END forloop
