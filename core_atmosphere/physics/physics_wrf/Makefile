.SUFFIXES: .F .o

all: dummy physics_wrf

dummy:
	echo "****** compiling physics_wrf ******"

OBJS = \
	libmassv.o                     \
	module_bep_bem_helper.o        \
	module_bl_gwdo.o               \
	module_bl_mynn.o               \
	module_bl_ysu.o                \
	module_cam_error_function.o    \
	module_cam_shr_kind_mod.o      \
	module_cam_support.o           \
	module_cu_gf.mpas.o            \
	module_cu_tiedtke.o            \
	module_cu_ntiedtke.o           \
	module_cu_kfeta.o              \
	module_mp_kessler.o            \
	module_mp_thompson.o           \
	module_mp_thompson_duplicated.o           \
	module_mp_tempo.o           \
	module_mp_thompson_cldfra3.o   \
	module_mp_wsm6.o               \
	module_ra_cam.o                \
	module_ra_cam_support.o        \
	module_ra_rrtmg_lw.o           \
	module_ra_rrtmg_sw.o           \
	module_ra_rrtmg_vinterp.o      \
	module_sf_bem.o                \
	module_sf_bep.o                \
	module_sf_bep_bem.o            \
	module_sf_sfcdiags.o           \
	module_sf_mynn.o               \
	module_sf_noahdrv.o            \
	module_sf_noahlsm.o            \
        module_sf_noahlsm_glacial_only.o \
        module_sf_noah_seaice.o        \
        module_sf_noah_seaice_drv.o    \
	module_sf_oml.o                \
	module_sf_sfclay.o             \
	module_sf_sfclayrev.o          \
	module_sf_urban.o              \
	bl_mynn_post.o                 \
	bl_mynn_pre.o                  \
	sf_mynn_pre.o

physics_wrf: $(OBJS)
	ar -ru ./../libphys.a $(OBJS)

# DEPENDENCIES:
module_bl_mynn.o: \
	bl_mynn_post.o \
	bl_mynn_pre.o

module_cam_support.o: \
	module_cam_shr_kind_mod.o

module_ra_cam.o: \
	module_cam_support.o \
	module_ra_cam_support.o

module_ra_rrtmg_lw.o: \
	module_ra_rrtmg_vinterp.o

module_ra_rrtmg_sw.o: \
	module_ra_rrtmg_lw.o \
	module_ra_rrtmg_vinterp.o

module_sf_bep.o: \
	module_bep_bem_helper.o \
	module_sf_urban.o

module_sf_bep_bem.o: \
	module_bep_bem_helper.o \
	module_sf_bem.o \
	module_sf_urban.o

module_sf_mynn.o: \
	sf_mynn_pre.o

module_sf_noahdrv.o: \
	module_sf_bem.o \
	module_sf_bep.o \
	module_sf_bep_bem.o \
	module_sf_noahlsm.o \
        module_sf_noahlsm_glacial_only.o \
	module_sf_urban.o

module_sf_noahlsm_glacial_only.o: \
        module_sf_noahlsm.o

module_sf_noah_seaice_drv.o: \
        module_sf_noah_seaice.o

module_sf_noah_seaice.o: \
        module_sf_noahlsm.o

clean:
	$(RM) *.f90 *.o *.mod
	@# Certain systems with intel compilers generate *.i files
	@# This removes them during the clean process
	$(RM) *.i

.F.o:
ifeq "$(GEN_F90)" "true"
	$(CPP) $(CPPFLAGS) $(COREDEF) $(CPPINCLUDES) $< > $*.f90
	$(FC) $(FFLAGS) -c $*.f90 $(FCINCLUDES) -I.. -I../physics_mmm -I../../../framework -I../../../external/esmf_time_f90
else
	$(FC) $(CPPFLAGS) $(COREDEF) $(FFLAGS) -c $*.F $(CPPINCLUDES) $(FCINCLUDES) -I.. -I../physics_mmm -I../../../framework -I../../../external/esmf_time_f90
endif
