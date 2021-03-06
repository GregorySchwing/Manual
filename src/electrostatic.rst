Intermolecular Energy and Virial Function (Electrostatic)
=========================================================

In this section, the virial and energy equation of electrostatic interaction for different potential function are discussed in details.

Ewald
-----
This option calculate electrostatic energy using standard *Ewald Summation Method*.

.. note:: Once this option is activated, it would override the the electrostatic calculation using ``VDW``, ``SHIFT``, and ``SWITCH`` functions.

``Potential Calculation``
  Coulomb interactions between atoms can be modeled as

  .. math::

    E(\texttt{Ewald}) = E_{real} + E_{reciprocal} + E_{self} + E_{correction}

  :math:`E_{real}`: Defines the short range electrostatic energy according to

  .. math::

    E_{real} = \frac{1}{4\pi \epsilon_0} \frac{1}{2} \sum_{i =1}^{N} \sum_{j = 1}^{N} q_i q_j  \frac{erfc(\alpha r_{ij})}{r_{ij}}
	
  , where :math:`\alpha` is ``Ewald`` separation parameter according to

  .. math::

    \alpha = \frac {\sqrt{-\log (Tolerance)}}{r_{cut}}
	
  , where ``Tolerance`` is a parameter, controlling the desired accuracy.

  :math:`E_{reciprocal}`: Defines the long range electrostatic energy according to,

  .. math::

    E_{reciprocal} = \frac{1}{\epsilon_0 V} \frac {1}{2} \sum_{\overrightarrow{k} \ne 0}^{} \frac {1}{\overrightarrow{k}^2}\exp\bigg(\frac {-\overrightarrow{k}^2}{4 \alpha^2}\bigg) \Bigg[ {\Big| R_{sum} \Big|}^2 + {\Big| I_{sum} \Big|}^2 \bigg]
	
  , where :math:`\overrightarrow{k}` is reciprocal vector, :math:`R_{sum}` and :math:`I_{sum}` are,

  .. math::

    R_{sum} = \sum_{i=1}^{N} q_i \cos \big(\overrightarrow{k}.\overrightarrow{x_i}\big)
	
  .. math::

    I_{sum} = \sum_{i=1}^{N} q_i \sin \big(\overrightarrow{k}.\overrightarrow{x_i}\big)
	
  :math:`E_{self}`: Defines the self energy according to,	
	
  .. math::

    E_{self} = -\frac{\alpha}{4\pi \epsilon_0 \sqrt{\pi}} \sum_{i=1}^{N} {q_i}^2
	
  :math:`E_{correction}`: Defines intra-molecule nonbonded energy,

  .. math::

    E_{correction} = -\frac{1}{4\pi \epsilon_0} \frac{1}{2} \sum_{j=1}^{N }\sum_{l =1}^{N_j} \sum_{m = 1}^{N_j} q_{j_l} q_{j_m}  \frac{erf(\alpha r_{j_l j_m})}{r_{j_l j_m}}
	
``Virial Calculation``
  Virial is basically the negative derivative of energy with respect to distance, multiplied by distance, Eq. 4. Coulomb force between atoms can be modeled as,

  .. math::

    W_{Ewald} = W_{real} + W_{reciprocal} 
	
  :math:`W_{real}` defines the short range electrostatic and :math:`W_{reciprocal}` defines the long range electrostatic force according to,

  .. math::

    W_{real} = \frac{1}{4\pi \epsilon_0} \frac{1}{2} \sum_{i =1}^{N} \sum_{j = 1}^{N} q_i q_j  \bigg[ \frac{erfc(\alpha r_{ij})}{r_{ij}} + \frac{2\alpha}{ \sqrt{\pi}} \exp(-\alpha^2 {r_{ij}}^2) \bigg] \times \frac{\overrightarrow{r_{ij}}}{{r_{ij}}^2}

  .. math::

    \begin{split}
		  W_{reciprocal} = \frac{1}{\epsilon_0 V} \frac {1}{2} \sum_{\overrightarrow{k} \ne 0}^{} \Bigg[\frac {1}{\overrightarrow{k}^2}\exp\bigg(\frac {-\overrightarrow{k}^2}{4 \alpha^2}\bigg) \bigg( {\Big| R_{sum} \Big|}^2 + {\Big| I_{sum} \Big|}^2 \bigg) \bigg(  1 - \frac{\overrightarrow{k}^2}{2\alpha^2} \bigg) \Bigg] +\\ 
      \sum_{i=1}^{N} \frac{1}{\epsilon_0 V}  \sum_{\overrightarrow{k} \ne 0}^{} \Bigg[ \frac {q_i}{\overrightarrow{k}^2}\exp\bigg(\frac {-\overrightarrow{k}^2}{4 \alpha^2}\bigg) \bigg[ I_{sum} \times\cos(\overrightarrow{k}.\overrightarrow{x_i})  - R_{sum} \times \sin(\overrightarrow{k}.\overrightarrow{x_i}) \bigg] \Bigg] \times \big( \overrightarrow{k}.\overrightarrow{r_{ic}} \big)
	  \end{split}
	
  , where :math:`\overrightarrow{r_{ic}}` is the vector between atom and the center of the mass of the molecule.

SHIFT
-----

This option forces the electrostatic energy to be zero at ``Rcut`` distance.

``Potential Calculation``
  Coulomb interactions between atoms can be modeled as
	
  .. math::
		
    E(\texttt{SHIFT}) = \frac{q_i q_j}{4\pi \epsilon_0} \Big( \frac{1}{r_{ij}} - \frac{1}{r_{cut}} \Big)
	
``Virial Calculation``
  Virial is basically the negative derivative of energy with respect to distance, multiplied by distance, Eq. 4. Coulomb force between atoms can be modeled as,

	.. math::

		W(\texttt{SHIFT}) = \frac{q_i q_j}{4\pi \epsilon_0} \Big( \frac{1}{r_{ij}} \times \frac{\overrightarrow{r_{ij}}}{{r_{ij}}^2} \Big)
	
SWITCH
------

This option in ``CHARMM`` or ``EXOTIC`` force field forces the electrostatic energy to be zero at ``Rcut`` distance.

``Potential Calculation``
  Coulomb interactions between atoms can be modeled as,
	
  .. math::

		E(\texttt{SWITCH}) = \frac{q_i q_j}{4\pi \epsilon_0} \bigg( \Big(\frac{r_{ij}}{r_{cut}} \Big)^2 - 1.0\bigg)^2 \frac{1}{r_{ij}}
	
``Virial Calculation``
  Virial is basically the negative derivative of energy with respect to distance, multiplied by distance, Eq. 4. Coulomb force between atoms can be modeled as,
	
  .. math::

		W(\texttt{SWITCH}) = \frac{q_i q_j}{4\pi \epsilon_0} \Bigg[ \bigg( \Big(\frac{r_{ij}}{r_{cut}} \Big)^2 - 1.0\bigg)^2 \frac{1}{{r_{ij}}^2} - \bigg( \frac{4}{{r_{cut}}^2} \bigg) \bigg( \Big(\frac{r_{ij}}{r_{cut}} \Big)^2 - 1.0\bigg) \Bigg] \times \frac{\overrightarrow{r_{ij}}}{r_{ij}}
	
SWITCH (MARTINI)
----------------

This option in ``MARTINI`` force field smoothly forces the potential energy to be zero at ``Rcut`` distance and starts modifying the potential at ``Rswitch = 0.0`` distance.

``Potential Calculation``
  Coulomb interactions between atoms can be modeled as,
	
  .. math::

		E(\texttt{SWITCH})=\frac{q_i q_j}{4\pi\epsilon_0\epsilon_1}\bigg(\frac{1}{r_{ij}}+\varphi_{1}(r_{ij})\bigg)
	
	
  , where :math:`\epsilon_1` is the dielectric constant, which in ``MARTINI`` force field is equal to 15.0 and :math:`\varphi_{\alpha}(r_{ij})` is defined in Eq. 13-16.
	
``Virial Calculation``
  Virial is basically the negative derivative of energy with respect to distance, multiplied by distance, Eq. 4. Coulomb force between atoms can be modeled as,
	
  .. math::

	  W(\texttt{SWITCH})=\frac{q_iq_j}{4\pi\epsilon_0\epsilon_1}\bigg(\frac{1}{{r_{ij}}^2}+d\varphi_1(r_{ij})\bigg)\times\frac{\overrightarrow{r_{ij}}}{r_{ij}}
  

  , where :math:`d\varphi_1 (r_{ij})` is defined in Eq. 18.

