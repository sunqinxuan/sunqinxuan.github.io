---
title: 'Incremental Calculation of RMSE'
date: 2025-03-31
permalink: /posts/research-journal-2025-03-31
excerpt: '...'
tags:
  - research journal
---

Let \\(y_i\\) and \\(\hat{y}_i\\) denote the groundtruth signal and predicted signal, respectively.
The root mean square error (RMSE) of the predicted signal containing \\(n\\) samples can be calculated as

$$
e_n=\sqrt{\frac{1}{n}
\sum^{n}_{i=1}(y_i-\hat{y}_i)^2
}
$$

It is evident that the RMSE corresponding to \\(n-1\\) samples is defined as

$$
e_{n-1}=\sqrt{\frac{1}{n-1}
\sum^{n-1}_{i=1}(y_i-\hat{y}_i)^2
}
$$

Then, after some simple calculations, it is obtained that

$$
e_n^2=
\frac{n-1}{n}e_{n-1}^2+
\frac{1}{n}(y_n-\hat{y}_n)^2
$$

It should be noted that special treatment is required when n=1.

The relevant C++ code implementation is provided below.

```c++
ret_t MicMagCompensator::update_rmse_sq()
{
    if (_mag_comp_storer.get_data_size<mic_mag_t>() == 0)
    {
        return ret_t::MIC_RET_FAILED;
    }
    if (_mag_comp_storer.get_data_size<mic_mag_t>() == 1)
    {
        mic_mag_t mag_comp, mag_truth;
        mic_nav_state_t nav_state;
        if (_mag_comp_storer.get_data<mic_mag_t>(_curr_time_stamp, mag_comp) &&
            _mag_truth_storer.get_data<mic_mag_t>(_curr_time_stamp, mag_truth) &&
            _mag_measure_storer.get_data<mic_nav_state_t>(_curr_time_stamp, nav_state))
        {
            matrix_3f_t R_nb = nav_state.attitude.matrix();
            vector_3f_t mag_b = R_nb.transpose() * mag_truth.vector.normalized() * mag_truth.value;
            _rmse_sq(0) = (mag_truth.value - mag_comp.value) * (mag_truth.value - mag_comp.value);
            _rmse_sq(1) = (mag_b(0) - mag_comp.vector(0)) * (mag_b(0) - mag_comp.vector(0));
            _rmse_sq(2) = (mag_b(1) - mag_comp.vector(1)) * (mag_b(1) - mag_comp.vector(1));
            _rmse_sq(3) = (mag_b(2) - mag_comp.vector(2)) * (mag_b(2) - mag_comp.vector(2));
            return ret_t::MIC_RET_SUCCESSED;
        }
        else
        {
            return ret_t::MIC_RET_FAILED;
        }
    }
    else
    {
        mic_mag_t mag_comp, mag_truth;
        mic_nav_state_t nav_state;
        if (_mag_comp_storer.get_data<mic_mag_t>(_curr_time_stamp, mag_comp) &&
            _mag_truth_storer.get_data<mic_mag_t>(_curr_time_stamp, mag_truth) &&
            _mag_measure_storer.get_data<mic_nav_state_t>(_curr_time_stamp, nav_state))
        {
            int N = _mag_comp_storer.get_data_size<mic_mag_t>();
            matrix_3f_t R_nb = nav_state.attitude.matrix();
            vector_3f_t mag_b = R_nb.transpose() * mag_truth.vector.normalized() * mag_truth.value;
            float64_t delta_t = (mag_truth.value - mag_comp.value) * (mag_truth.value - mag_comp.value);
            float64_t delta_x = (mag_b(0) - mag_comp.vector(0)) * (mag_b(0) - mag_comp.vector(0));
            float64_t delta_y = (mag_b(1) - mag_comp.vector(1)) * (mag_b(1) - mag_comp.vector(1));
            float64_t delta_z = (mag_b(2) - mag_comp.vector(2)) * (mag_b(2) - mag_comp.vector(2));
            _rmse_sq(0) = _rmse_sq(0) * (N - 1) / N + delta_t / N;
            _rmse_sq(1) = _rmse_sq(1) * (N - 1) / N + delta_x / N;
            _rmse_sq(2) = _rmse_sq(2) * (N - 1) / N + delta_y / N;
            _rmse_sq(3) = _rmse_sq(3) * (N - 1) / N + delta_z / N;
            return ret_t::MIC_RET_SUCCESSED;
        }
        else
        {
            return ret_t::MIC_RET_FAILED;
        }
    }
}

```

