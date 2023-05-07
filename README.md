$-----------------------------------------------------------------------------
$
$ Example provided by Iñaki (LSTC) and Hossein Mohammadi (McGill University)
$
$ E-Mail: info@dynamore.de
$ Web: http://www.dynamore.de
$
$ Copyright, 2015 DYNAmore GmbH
$ Copying for non-commercial usage allowed if
$ copy bears this notice completely.
$
$X------------------------------------------------------------------------------
$X
$X 1. Run file as is.
$X    Requires LS-DYNA MPP Dev 117000 (or higher) with double precision 
$X
$X------------------------------------------------------------------------------
$# UNITS: (g/cm/s)
$X------------------------------------------------------------------------------
$X
*KEYWORD
*TITLE
ICFD Blood flow
*INCLUDE
mesh.k
$---+----1----+----2----+----3----+----4----+----5----+----6----+----7----+----8
$                                                                              $
$                             PARAMETERS                                       $
$                                                                              $
$---+----1----+----2----+----3----+----4----+----5----+----6----+----7----+----8
*PARAMETER
R    t_end       0.5
R dt_fluid      5e-5
R  dt_plot     0.005
Rp_init       126656
$
$--- Fluid
$
$ Carreau model for blood flow
Rrho_fluid     1.059
R mu_fluid     0.035
R mu_o          0.56
R n           0.3568
R lambda       3.313
$ Windkessel parameters
R ra4          45000
R ca4         8.e-08
R ram4        75000.
R cv4          3.e-6
R rv4         70000.
R ra5         100000
R ca5        1.0E-07
R ram5       160000.
R cv5          4.e-6
R rv5         40000.
R ra6         25000.
R ca6        6.0E-07
R ram6        40000.
R cv6         1.8e-5
R rv6          9500.
$---+----1----+----2----+----3----+----4----+----5----+----6----+----7----+----8
$                                                                              $
$                           ICFD CONTROL CARDS                                 $
$                                                                              $
$---+----1----+----2----+----3----+----4----+----5----+----6----+----7----+----8
*ICFD_CONTROL_MESH
      1.05
*ICFD_CONTROL_TIME
$#     ttm        dt       cfl
    &t_end &dt_fluid
*ICFD_CONTROL_TURBULENCE
$#    model  Epsilon   k        omega     Cmu      C1e      C2e      Sigmae   Kmin
       2     1.2      0.1      0.1       0.09     1.4      1.8      1.2      0.01
$---+----1----+----2----+----3----+----4----+----5----+----6----+----7----+----8
$                                                                              $
$                       ICFD PARTS/ SECTION/ MATERIAL                          $
$                                                                              $
$---+----1----+----2----+----3----+----4----+----5----+----6----+----7----+----8
*ICFD_MAT
$#     mid       flg        ro       vis      
         1         1&rho_fluid &mu_fluid    

         2
*ICFD_MODEL_NONNEWT
         2        2
     &mu_o       &n  &mu_fluid   &lambda
*ICFD_PART
$#     pid     secid       mid
         1         1         1
*ICFD_PART
$#     pid     secid       mid
         2         1         1
*ICFD_PART
$#     pid     secid       mid
         3         1         1
*ICFD_PART
$#     pid     secid       mid
         4         1         1
*ICFD_PART
$#     pid     secid       mid
         5         1         1
*ICFD_PART
$#     pid     secid       mid
         6         1         1
*ICFD_PART_VOL
$#     pid     secid       mid
         7         1         1
$#   spid1     spid2     spid3     spid4     spid5     spid6     spid7     spid8
         1         2         3         4         5         6 
*ICFD_SECTION
$#     sid
         1
$---+----1----+----2----+----3----+----4----+----5----+----6----+----7----+----8
$                                                                              $
$                    ICFD BOUNDARY/INITIAL/LOAD CONDITIONS                     $
$                                                                              $
$---+----1----+----2----+----3----+----4----+----5----+----6----+----7----+----8
*ICFD_BOUNDARY_PRESCRIBED_PRE
         1         2
*ICFD_BOUNDARY_PRESCRIBED_PRE
         2         1
*ICFD_BOUNDARY_NONSLIP
$#     pid
         3
*ICFD_BOUNDARY_WINDKESSEL
$#     pid     wtype        r1        c1        r2
         4         3      &ra4      &ca4     &ram4
$#  p2lcid        c2        r3
                &cv4      &rv4
*ICFD_BOUNDARY_WINDKESSEL
$#     pid     wtype        r1        c1        r2
         5         3      &ra5      &ca5     &ram5
$#  p2lcid        c2        r3
                &cv5      &rv5
*ICFD_BOUNDARY_WINDKESSEL
$#     pid     wtype        r1        c1        r2
         6         3      &ra6      &ca6     &ram6
$#  p2lcid        c2        r3
                &cv6      &rv6
*ICFD_INITIAL
         0         0         0         0         0   &p_init
*DEFINE_CURVE
1,,,1,-0.0243,
0,12239
0.0243,126656.2705
0.0328,129456.0407
0.0412,133189.0676
0.0496,137322.0617
0.058,141321.7334
0.0665,146654.629
0.0749,150654.3007
0.0833,153320.7485
0.0917,155987.1963
0.1,157320.4202
0.109,158653.6441
0.117,159986.868
0.125,159986.868
0.134,159986.868
0.142,159986.868
0.151,159986.868
0.159,158653.6441
0.168,155987.1963
0.176,153320.7485
0.184,150654.3007
0.193,147987.8529
0.201,145321.4051
0.21,143988.1812
0.218,141321.7334
0.227,138655.2856
0.235,135988.8378
0.243,134655.6139
0.252,134655.6139
0.26,135988.8378
0.269,138655.2856
0.277,139988.5095
0.286,142654.9573
0.294,143988.1812
0.302,143988.1812
0.311,142654.9573
0.319,141321.7334
0.328,138655.2856
0.336,137322.0617
0.344,135988.8378
0.353,134655.6139
0.361,133322.39
0.37,133055.7452
0.378,132922.4228
0.387,132655.7781
0.395,131589.1989
0.403,130122.6526
0.412,129189.3959
0.42,128656.1064
0.429,128389.4616
0.437,127989.4944
0.446,127589.5272
0.454,127189.5601
0.462,126789.5929
0.471,125989.6586
0.479,125189.7242
0.488,124389.7899
0.496,123589.8555
0.505,123189.8884
*DEFINE_CURVE
2
0,0
1000,0
$---+----1----+----2----+----3----+----4----+----5----+----6----+----7----+----8
$                                                                              $
$                            ICFD MESH KEYWORDS                                $
$                                                                              $
$---+----1----+----2----+----3----+----4----+----5----+----6----+----7----+----8
*MESH_BL
         3         1
*MESH_BL_SYM
         1
*MESH_BL_SYM
         2
*MESH_BL_SYM
         4
*MESH_BL_SYM
         5
*MESH_BL_SYM
         6
*MESH_VOLUME
$#   volid
        10
$#    pid1      pid2      pid3      pid4      pid5      pid6      pid7      pid8
         1         2         3         4         5         6
$---+----1----+----2----+----3----+----4----+----5----+----6----+----7----+----8
$                                                                              $
$                             DATABASE (OUTPUT)                                $
$                                                                              $
$---+----1----+----2----+----3----+----4----+----5----+----6----+----7----+----8
*ICFD_DATABASE_FLUX
         1
*ICFD_DATABASE_FLUX
         2
*ICFD_DATABASE_FLUX
         4
*ICFD_DATABASE_FLUX
         5
*ICFD_DATABASE_FLUX
         6
*ICFD_DATABASE_TIMESTEP
         1
*DATABASE_BINARY_D3PLOT
$#      dt      lcdt      beam     npltc    psetid
&dt_plot
*END

