========================================
Egyszerű folyamat létrehozása, futtatása
========================================

1. lépés - BPMN létrehozása
---------------------------

A projekt ``src/main/resources`` könyvtárára jobb klikk, *New > BPMN2 process*, File name: ``myprocess``, [Finish]

Vegyünk fel egy *Script Taskot*, és egy *End Eventet*, húzzuk be a *Sequence Flow*-t. Az eredmény kb. így néz ki:

.. image:: https://github.com/reedcourty/symosim-know-how/raw/master/simple-process/figures/bpmn.png

Mentsük el a folyamatot!

Az ``src/main/java`` könyvtárra jobb klikk, *New > Package*, Name: pl. com.github.reedcourty.symosim_know_how.simple_process

http://docs.oracle.com/javase/tutorial/java/package/namingpkgs.html

Ezután jobb klikk az új package-re, *New > Class*, Name: ``ProcessMain``

Tartalma:

::

   package com.github.reedcourty.symosim_know_how.simple_process;

   import org.drools.KnowledgeBase;
   import org.drools.builder.KnowledgeBuilder;
   import org.drools.builder.KnowledgeBuilderFactory;
   import org.drools.builder.ResourceType;
   import org.drools.io.ResourceFactory;
   import org.drools.runtime.StatefulKnowledgeSession;

   import org.drools.logger.KnowledgeRuntimeLogger;
   import org.drools.logger.KnowledgeRuntimeLoggerFactory;

   public class ProcessMain {

      public static final void main(String[] args) throws Exception {
         // load up the knowledge base
         KnowledgeBase kbase = readKnowledgeBase();
         StatefulKnowledgeSession ksession = kbase.newStatefulKnowledgeSession();
         KnowledgeRuntimeLogger logger = KnowledgeRuntimeLoggerFactory.newThreadedFileLogger(ksession, "test", 1000);
         
         ksession.startProcess("com.github.reedcourty.symosim_know_how.simple_process.bpmn.myprocess");
         
         logger.close();
      }
   
      private static KnowledgeBase readKnowledgeBase() throws Exception {
         KnowledgeBuilder kbuilder = KnowledgeBuilderFactory.newKnowledgeBuilder();
         kbuilder.add(ResourceFactory.newClassPathResource("myprocess.bpmn"), ResourceType.BPMN2);
         return kbuilder.newKnowledgeBase();
      }
      
   }

Ezután a ``myprocess.bpmn``-t megnyitva, a *Properties* fülön ``Id``-nak adjuk meg az előző osztályban szereplő ``com.github.reedcourty.symosim_know_how.simple_process.bpmn.myprocess`` értéket.

Ha mindent jól csináltunk, akkor a ``ProcessMain``-t futtatva nem kapunk hibát.