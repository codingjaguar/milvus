<img src="https://repository-images.githubusercontent.com/208728772/998c09ca-cfa6-4c01-ac75-3dfad7f4862b" alt="milvus banner">

<div class="column" align="middle">
  <a href="https://github.com/milvus-io/milvus/blob/master/LICENSE"><img height="20" src="https://img.shields.io/github/license/milvus-io/milvus" alt="license"/></a>
  <a href="https://milvus.io/docs/install_standalone-docker.md"><img src="https://img.shields.io/docker/pulls/milvusdb/milvus" alt="docker-pull-count"/></a>
  <a href="https://discord.com/invite/8uyFbECzPX"><img height="20" src="https://img.shields.io/badge/Discord-%235865F2.svg?style=for-the-badge&logo=discord&logoColor=white" alt="discord"/></a>
</div>

## What is Milvus?

[Milvus](https://milvus.io/) is a high-performance vector database built for scale. It is used by AI applications to organize and search through large amounts of unstructured data, such as text and images.

Milvus is implemented with Go and C++ and employs CPU/GPU instruction-level optimization for vector search efficiency. It has a [fully-distributed architecture on K8s](https://milvus.io/docs/overview.md#What-Makes-Milvus-so-Scalable) to handle tens of thousands of search queries on billions of vectors, scale horizontally and maintain data freshness by processing streaming data updates in real-time. For smaller use cases, Milvus also supports [Standalone mode](https://milvus.io/docs/install_standalone-docker.md) that can run on Docker. In addition, [Milvus Lite](https://milvus.io/docs/milvus_lite.md) is a lightweight version suitable for quickstart in python with simply `pip install`.

Milvus is also available as a hosted service on [Zilliz Cloud with free trial](https://cloud.zilliz.com/signup).

The Milvus open-source project is
under [LF AI & Data Foundation](https://lfaidata.foundation/projects/milvus/), distributed with [Apache 2.0](https://github.com/milvus-io/milvus/blob/master/LICENSE) License.

## Quickstart

```python
$ pip install -U pymilvus
```

This installs `pymilvus`, a Python SDK for Milvus. It also includes Milvus Lite for quickstart with a local vector database. Simply instantiate a client with a local file name to persist all data:

```python
from pymilvus import MilvusClient
client = MilvusClient("milvus_demo.db")
```

For [Milvus Standalone on Docker](https://milvus.io/docs/install_standalone-docker.md) and [Milvus Distributed on Kubernetes](https://milvus.io/docs/install_cluster-milvusoperator.md), specify the [URI and Token](https://milvus.io/api-reference/pymilvus/v2.4.x/MilvusClient/Client/MilvusClient.md) instead to connect to the Milvus server:

```python
client = MilvusClient(uri="http://localhost:19530", token="root:Milvus")
```

With the client, you can create collection:
```python
client.create_collection(
    collection_name="demo_collection",
    dimension=768,  # The vectors we will use in this demo has 768 dimensions
)
```

 ingest data:
```python
res = client.insert(collection_name="demo_collection", data=data)
```

and perform vector search:

```python
query_vectors = embedding_fn.encode_queries(["Who is Alan Turing?", "What is AI?"])
res = client.search(
    collection_name="demo_collection",  # target collection
    data=query_vectors,  # a list of query vectors, supports batch search req
    limit=2,  # number of returned results (topK)
    output_fields=["vector", "text", "subject"],  # specifies fields to return
)

```

## Why Milvus

Milvus is designed to handle vectors, which are numerical representations of unstructured data, together with other scalar data types such as integers, strings, and JSON objects. Users can store scalar data with vectors to conduct vector search with metadata filtering.

* **High performance and horizontal scalability**:
  *  Milvus provides sub-ten millisecond search latency at scale. For detailed performance, checkout [VectorDBBench](https://zilliz.com/vector-database-benchmark-tool).

* **Intuitive API and rich SDK support**:
  * Milvus implements easy-to-use API for vector search, data query, ingestion, update and collection management.
In addition to RESTful and gRPC API, Milvus also provides SDK in [Python](https://github.com/milvus-io/pymilvus), [Java](https://github.com/milvus-io/milvus-sdk-java), [Go](https://github.com/milvus-io/milvus-sdk-go), [C++](https://github.com/milvus-io/milvus-sdk-cpp), [Node.js](https://github.com/milvus-io/milvus-sdk-node), [Rust](https://github.com/milvus-io/milvus-sdk-rust), [C#](https://github.com/milvus-io/milvus-sdk-csharp)


* **High availability**:
  * Milvus is highly available thanks to its fully-distributed architecture. To learn more, check [Milvus Architecture Overview](https://milvus.io/docs/architecture_overview.md).
**Various Index Types Support**

* **Efficient Metadata Filtering**
  * Milvus has various optimizations to make vector search efficient when combined with metadata filtering, especially at high filtering rate, where post-filtering doesn't work, [VectorDBBench](https://zilliz.com/vector-database-benchmark-tool) shows the performance.

* **Hardware Acceleration**
  * Milvus implements instruction-level acceleration to speed up vector search performance, and supports GPU index such as NVIDIA [CAGRA](https://github.com/rapidsai/raft).

* **Hybrid Search**
  * Users can define up to 10 vector fields in a single collection. Milvus supports hybrid search on multiple vector columns, merge and rerank with Reciprocal Rank Fusion (RRF) and Weighted Scoring. For details, refer to [Hybrid Search](https://milvus.io/docs/multi-vector-search.md).

Milvus is trusted by AI developers in startups and enterprises to develop applications such as text and image search, Retrieval-Augmented Generation (RAG), and recommendation systems. Milvus powers mission-critical business for users including Salesforce, PayPal, Shopee, Airbnb, eBay, NVIDIA, IBM, AT&T, LINE, and ROBLOX.

## Demos and Tutorials 


| Tutorial | Use Case | Related Milvus Features | 
| -------- | -------- | --------- |
| [Build RAG with Milvus](build-rag-with-milvus.md) |  RAG | vector search |
| [Multimodal RAG with Milvus](multimodal_rag_with_milvus.md) | RAG | vector search, dynamic field |
| [Image Search with Milvus](image_similarity_search.md) | Semantic Search | vector search, dynamic field |
| [Hybrid Search with Milvus](hybrid_search_with_milvus.md) | Hybrid Search | hybrid search, multi vector, dense embedding, sparse embedding |
| [Multimodal Search using Multi Vectors](multimodal_rag_with_milvus.md) | Semantic Search | multi vector, hybrid search |
| [Recommender System](recommendation_system.md) | Recommendation System | vector search |
| [Video Similarity Search](video_similarity_search.md) | Semantic Search | vector search |
| [Audio Similarity Search](audio_similarity_search.md) | Semantic Search | vector search |
| [DNA Classification](dna_sequence_classification.md) | Classification | vector search |
| [Graph RAG with Milvus](graph_rag_with_milvus.md) | RAG | vector search |
| [Contextual Retrieval with Milvus](contextual_retrieval_with_milvus.md) | RAG, Semantic Search | vector search |
| [HDBSCAN Clustering with Milvus](hdbscan_clustering_with_milvus.md) | Clustering | vector search |
| [Use ColPali for Multi-Modal Retrieval with Milvus](use_ColPali_with_milvus.md) | RAG, Semantic Search | vector search |
| [Vector Visualization](vector_visualization.md) | Data Visualization | vector search |

<table>
  <tr>
    <td width="30%">
      <a href="https://milvus.io/milvus-demos">
        <img src="https://assets.zilliz.com/image_search_59a64e4f22.gif" />
      </a>
    </td>
    <td width="30%">
<a href="https://milvus.io/milvus-demos">
<img src="https://assets.zilliz.com/qa_df5ee7bd83.gif" />
</a>
    </td>
    <td width="30%">
<a href="https://milvus.io/milvus-demos">
<img src="https://assets.zilliz.com/mole_search_76f8340572.gif" />
</a>
    </td>
  </tr>
  <tr>
    <th>
      <a href="https://milvus.io/milvus-demos">Image Search</a>
    </th>
    <th>
      <a href="https://milvus.io/milvus-demos">RAG</a>
    </th>
    <th>
      <a href="https://milvus.io/milvus-demos">Drug Discovery</a>
    </th>
  </tr>
</table>

## Documentation

For guidance on installation, development, deployment, and administration, check out [Milvus Docs](https://milvus.io/docs). For technical milestones and enhancement proposals, check out [milvus confluence](https://wiki.lfaidata.foundation/display/MIL/Milvus+Home)

## Build Milvus from Source Code

Check the requirements first.

Linux systems (Ubuntu 20.04 or later recommended):
```bash
go: >= 1.21
cmake: >= 3.26.4
gcc: 9.5
python: > 3.8 and  <= 3.11
```

MacOS systems with x86_64 (Big Sur 11.5 or later recommended):
```bash
go: >= 1.21
cmake: >= 3.26.4
llvm: >= 15
python: > 3.8 and  <= 3.11
```

MacOS systems with Apple Silicon (Monterey 12.0.1 or later recommended):
```bash
go: >= 1.21 (Arch=ARM64)
cmake: >= 3.26.4
llvm: >= 15
python: > 3.8 and  <= 3.11
```

Clone Milvus repo and build.

```bash
# Clone github repository.
$ git clone https://github.com/milvus-io/milvus.git

# Install third-party dependencies.
$ cd milvus/
$ ./scripts/install_deps.sh

# Compile Milvus.
$ make
```

For the full story, see [developer's documentation](https://github.com/milvus-io/milvus/blob/master/DEVELOPMENT.md).

## Contributing

Contributions to Milvus are welcome from everyone. See [Guidelines for Contributing](https://github.com/milvus-io/milvus/blob/master/CONTRIBUTING.md) for details on submitting patches and the contribution workflow. See our [community repository](https://github.com/milvus-io/community) to learn about our governance and access more community resources.

## Documentation

For guidance on installation, development, deployment, and administration, check out [Milvus Docs](https://milvus.io/docs). For technical milestones and enhancement proposals, check out [milvus confluence](https://wiki.lfaidata.foundation/display/MIL/Milvus+Home)

### SDK

The implemented SDK and its API documentation are listed below:

- [PyMilvus SDK](https://github.com/milvus-io/pymilvus)
- [Java SDK](https://github.com/milvus-io/milvus-sdk-java)
- [Go SDK](https://github.com/milvus-io/milvus-sdk-go)
- [Cpp SDK](https://github.com/milvus-io/milvus-sdk-cpp)(under development)
- [Node SDK](https://github.com/milvus-io/milvus-sdk-node)
- [Rust SDK](https://github.com/milvus-io/milvus-sdk-rust)(under development)
- [CSharp SDK](https://github.com/milvus-io/milvus-sdk-csharp)(under development)

### Attu

Attu provides an intuitive and efficient GUI for Milvus.

- [Quick start](https://github.com/zilliztech/milvus-insight#quick-start)

## Community

Join the Milvus community on [Discord](https://discord.gg/8uyFbECzPX) to share your suggestions, advice, and questions with our engineering team.

You can also check out our [FAQ page](https://milvus.io/docs/performance_faq.md) to discover solutions or answers to your issues or questions.

Subscribe to Milvus mailing lists:

- [Technical Steering Committee](https://lists.lfai.foundation/g/milvus-tsc)
- [Technical Discussions](https://lists.lfai.foundation/g/milvus-technical-discuss)
- [Announcement](https://lists.lfai.foundation/g/milvus-announce)

Follow Milvus on social media:

- [Medium](https://medium.com/@milvusio)
- [X](https://twitter.com/milvusio)
- [Youtube](https://www.youtube.com/channel/UCMCo_F7pKjMHBlfyxwOPw-g)

## Reference

Reference to cite when you use Milvus in a research paper:

```
@inproceedings{2021milvus,
  title={Milvus: A Purpose-Built Vector Data Management System},
  author={Wang, Jianguo and Yi, Xiaomeng and Guo, Rentong and Jin, Hai and Xu, Peng and Li, Shengjun and Wang, Xiangyu and Guo, Xiangzhou and Li, Chengming and Xu, Xiaohai and others},
  booktitle={Proceedings of the 2021 International Conference on Management of Data},
  pages={2614--2627},
  year={2021}
}

@article{2022manu,
  title={Manu: a cloud native vector database management system},
  author={Guo, Rentong and Luan, Xiaofan and Xiang, Long and Yan, Xiao and Yi, Xiaomeng and Luo, Jigao and Cheng, Qianya and Xu, Weizhi and Luo, Jiarui and Liu, Frank and others},
  journal={Proceedings of the VLDB Endowment},
  volume={15},
  number={12},
  pages={3548--3561},
  year={2022},
  publisher={VLDB Endowment}
}
```

## Acknowledgments

Milvus adopts dependencies from the following:

- Thanks to [FAISS](https://github.com/facebookresearch/faiss) for the excellent search library.
- Thanks to [etcd](https://github.com/coreos/etcd) for providing great open-source key-value store tools.
- Thanks to [Pulsar](https://github.com/apache/pulsar) for its wonderful distributed pub-sub messaging system.
- Thanks to [Tantivy](https://github.com/quickwit-oss/tantivy) for its full-text search engine library written in Rust.
- Thanks to [RocksDB](https://github.com/facebook/rocksdb) for the powerful storage engines.


