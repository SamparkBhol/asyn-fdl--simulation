# 🌀 Asynchronous Federated Learning Simulation

## 📖 Overview
A serial framework for simulating **asynchronous federated training** of NN-based classifiers.  

Currently supports:
- **MNIST** → CNN  
- **CIFAR-10** → ResNet-18  

---

## ⚡ Asynchronous Federated Learning
Implements **buffered asynchronous aggregation** ([FedBuff](https://arxiv.org/abs/2106.06639)) for **cross-silo settings**.

### Workflow
1. Server initializes and broadcasts the global model.  
2. Clients train locally and send updates asynchronously.  
3. Clients download the latest global model after each update.  
4. Server updates the global model after receiving a buffer of updates.  

---

## 🔀 Training Modes

### Asynchronous
- Clients send updates independently.  
- Optional correction ([paper](https://arxiv.org/abs/2405.10123)) balances heterogeneous update rates.  

### Synchronous (FedAvg)
- Server samples a subset of clients each round.  
- Clients update synchronously.  
- Based on [FedAvg](https://arxiv.org/abs/1602.05629).  

---

## 👥 Client Update Model
- Client update interval:  
  \[
  t_i \sim \text{Exp}(\lambda_i)
  \]  

- Client update rate:  
  \[
  \lambda_i \sim \text{Log-normal}(0, \sigma^2)
  \]  

This models **heterogeneous client speeds**.  

---

## 🚀 Usage

Run:
python main.py --args <args>

## Arguments

| Argument            | Type   | Default | Description                                                                 |
|---------------------|--------|---------|-----------------------------------------------------------------------------|
| `--num_clients`     | int    | 10      | Number of clients                                                           |
| `--dataset`         | str    | mnist   | Dataset: MNIST or CIFAR-10                                                  |
| `--train_batch_size`| int    | 64      | Local training batch size                                                   |
| `--test_batch_size` | int    | 32      | Test batch size                                                             |
| `--Delta`           | int    | 3       | Local updates per aggregation (async) / sampled clients (FedAvg)            |
| `--lr`              | float  | 0.01    | Local learning rate                                                         |
| `--num_local_steps` | int    | 100     | Local SGD steps                                                             |
| `--dirichlet_alpha` | float  | 1.0     | Controls dataset heterogeneity                                              |
| `--mode`            | str    | async   | Training mode: sync or async                                                |
| `--correction`      | bool   | False   | Apply correction scheme                                                     |
| `--client_rate_std` | float  | 0.1     | Std. dev. for client update rates                                           |
| `--T_train`         | float  | 5.0     | Total training time                                                         |

---

## 📚 References
- **FedAvg** → [Communication-Efficient Learning of Deep Networks from Decentralized Data](https://arxiv.org/abs/1602.05629)  
- **FedBuff** → [Efficient Federated Learning via Buffered Asynchronous Aggregation](https://arxiv.org/abs/2106.06639)  
- **Async FL Correction** → [Balancing Heterogeneous Client Update Frequencies in Async Federated Learning](https://arxiv.org/abs/2405.10123)  

