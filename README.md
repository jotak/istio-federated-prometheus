# Istio Federated Prometheus

Some templates &amp; Makefile to setup federated Prometheus for Istio.

These steps assume Istio 1.5 (or above) is already installed in namespace istio-system.

## Production-ready Istio #1

Built after this guide: https://istio.io/docs/ops/best-practices/observability/#using-prometheus-for-production-scale-monitoring

```
make istio-prod-ready-1
```

This is going to:
- Amend the existing prometheus from istio-system (reducing metrics retention, setting up config with rewiting rules)
- Create a "master" prometheus, with longer retention, and that reads from the former.

Note: there is a known issue with this guide, about [summing metrics before getting their rates](https://www.robustperception.io/rate-then-sum-never-sum-then-rate), also mentioned here: https://discuss.istio.io/t/feedback-requested-production-monitoring-with-prometheus/5685.

## Exposing "master" Prometheus

```
make expose
```

Exposes the "master" prometheus via port-forward to http://localhost:9091

## Patching Kiali configuration

```
make patch-kiali
```

Assuming Kiali is deployed with [kiali-operator](https://kiali.io/documentation/getting-started/#_install_kiali_latest), will amend Kiali configuration to point to "master" prometheus.

## Production-ready Istio #2

Similar to the former, but avoids the pitfall of summing before rating (in other words: without a bug).

But it comes with a serious drawback: consumers (Grafana, Kiali ...) would be broken. So Grafana dashboards would need to be updated. And for Kiali, that's a different story, no fix available yet.

```
make istio-prod-ready-2
```
